<script>
    import {
        PencilSimple,
        Play,
        Pause,
        ClockClockwise,
        CaretUp,
        CaretDown,
        CheckFat,
    } from "phosphor-svelte";

    import AlarmSound from "/analogAlarm.mp3";
    const AlarmAudio = new Audio(AlarmSound);

    let modes = ["work", "break", "long"];
    let modeTimes = [1500, 300, 600];
    let currentModeTime = modeTimes[0];
    let currentMode = modes[0];

    let modeColors = ["#FF3131", "#0077FF", "#00CA36"];

    function updateMode(mode) {
        currentModeTime = modeTimes[mode];
        currentMode = modes[mode];
        timeRemaining = currentModeTime;
        document.documentElement.style.setProperty(
            "--accent",
            modeColors[mode],
        );
    }

    let timer;
    let timeRemaining = currentModeTime;
    let isTimerRunning = false;
    let isTimerFinished = true;

    function startPomo() {
        if (!isTimerRunning && !isEditOpen) {
            isTimerRunning = true;
            isTimerFinished = false;
            timer = setInterval(() => {
                if (timeRemaining > 0) {
                    timeRemaining -= 1;
                }
            }, 1000);
        }
    }

    $: if (timeRemaining < 1) {
        resetPomo();
        AlarmAudio.play();
    }

    function pausePomo() {
        if (isTimerRunning) {
            clearInterval(timer);
            isTimerRunning = false;
        }
    }

    function resetPomo() {
        clearInterval(timer);
        isTimerRunning = false;
        isTimerFinished = true;
        timeRemaining = currentModeTime;
    }

    function formatTime(time) {
        let mins = Math.floor(time / 60);
        let secs = time % 60;
        let formatTime =
            String(mins).padStart(2, "0") + ":" + String(secs).padStart(2, "0");
        return formatTime;
    }

    let isEditOpen = false;

    function openCloseEdit() {
        if (isEditOpen) {
            isEditOpen = false;
        } else {
            isEditOpen = true;
        }
    }

    function updateCurrentTime(modif) {
        if (timeRemaining + modif > 0) {
            timeRemaining = timeRemaining + modif;
        }
    }
</script>

{#if isEditOpen}
    <div class="edit-menu">
        <div class="toggleBx">
            <button
                class={currentMode == "work"
                    ? "toggle-item toggle-active"
                    : "toggle-item"}
                on:click={() => {
                    updateMode(0);
                }}
            >
                work
            </button>
            <button
                class={currentMode == "break"
                    ? "toggle-item toggle-active"
                    : "toggle-item"}
                on:click={() => {
                    updateMode(1);
                }}
            >
                break
            </button>
            <button
                class={currentMode == "long"
                    ? "toggle-item toggle-active"
                    : "toggle-item"}
                on:click={() => {
                    updateMode(2);
                }}
            >
                long
            </button>
        </div>

        <div class="edit-timeBx">
            <div class="edit-controls">
                <button
                    on:click={() => {
                        updateCurrentTime(60);
                    }}><CaretUp size={28} weight="fill"></CaretUp></button
                >
                <button
                    on:click={() => {
                        updateCurrentTime(-60);
                    }}><CaretDown size={28} weight="fill"></CaretDown></button
                >
            </div>
            <div class="edit-time">{formatTime(timeRemaining)}</div>
            <div class="edit-controls">
                <button
                    on:click={() => {
                        updateCurrentTime(1);
                    }}><CaretUp size={28} weight="fill"></CaretUp></button
                >
                <button
                    on:click={() => {
                        updateCurrentTime(-1);
                    }}><CaretDown size={28} weight="fill"></CaretDown></button
                >
            </div>
        </div>

        <button class="edit-done" on:click={openCloseEdit}>
            <CheckFat weight="fill" size={14}></CheckFat>
            DONE
        </button>
    </div>
{/if}

<div class="pomodoro">
    {#if !isTimerFinished}
        <button class="pomo-button" aria-label="button" on:click={resetPomo}>
            <ClockClockwise weight="fill" size={24}></ClockClockwise>
        </button>
    {:else}
        <button
            class="pomo-button"
            aria-label="button"
            on:click={openCloseEdit}
        >
            <PencilSimple weight="fill" size={24}></PencilSimple>
        </button>
    {/if}
    <div class="pomo-timeBx">
        <div class="pomo-attachment">{currentMode}</div>
        <div class="pomo-time-bg">
            <div class="pomo-time">{formatTime(timeRemaining)}</div>
        </div>
    </div>
    {#if isTimerRunning}
        <button class="pomo-button" aria-label="button" on:click={pausePomo}>
            <Pause class="play-pomo" weight="fill" size={24}></Pause>
        </button>
    {:else}
        <button class="pomo-button" aria-label="button" on:click={startPomo}>
            <Play class="play-pomo" weight="fill" size={24}></Play>
        </button>
    {/if}
</div>
