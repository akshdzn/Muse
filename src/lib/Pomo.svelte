<script>
    import { PencilSimple, Play, Pause, ClockClockwise } from "phosphor-svelte";

    let modes = ["work", "break", "long"];
    let modeTimes = [1500, 300, 600];
    let currentModeTime = modeTimes[0];
    let currentMode = modes[0];

    function updateMode(mode) {
        currentModeTime = modeTimes[mode];
        currentMode = modes[mode];
        timeRemaining = currentModeTime;
    }

    let timer;
    let timeRemaining = currentModeTime;
    let isTimerRunning = false;
    let isTimerFinished = true;

    function startPomo() {
        if (!isTimerRunning) {
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
        <div class="pomo-attachment">work</div>
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
