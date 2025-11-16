<script>
    import { PencilSimple, Play, Pause, ClockClockwise } from "phosphor-svelte";

    const workTime = 1500;

    let timer;
    let timeRemaining = workTime;
    let isTimerRunning = false;

    function startPomo() {
        if (!isTimerRunning) {
            isTimerRunning = true;
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
        if (isTimerRunning) {
            clearInterval(timer);
            isTimerRunning = false;
            timeRemaining = workTime;
        }
    }

    function formatTime(time) {
        let mins = Math.floor(time / 60);
        let secs = time % 60;
        let formatTime =
            String(mins).padStart(2, "0") + ":" + String(secs).padStart(2, "0");
        return formatTime;
    }
</script>

<div class="pomodoro">
    {#if isTimerRunning}
        <button class="pomo-button" aria-label="button" on:click={resetPomo}>
            <ClockClockwise weight="fill" size={24}></ClockClockwise>
        </button>
    {:else}
        <button class="pomo-button" aria-label="button">
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
