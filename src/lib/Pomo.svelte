<script>
    import { PencilSimple, Play } from "phosphor-svelte";
</script>

<div class="pomodoro">
    <button class="pomo-button" aria-label="button">
        <PencilSimple weight="fill" size={24}></PencilSimple>
    </button>
    <div class="pomo-timeBx">
        <div class="pomo-attachment">work</div>
        <div class="pomo-time">25:00</div>
    </div>
    <button class="pomo-button" aria-label="button">
        <Play class="play-pomo" weight="fill" size={24}></Play>
    </button>
</div>
