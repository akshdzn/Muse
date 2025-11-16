<script>
    import { MusicNote, GithubLogo } from "phosphor-svelte";
    import Music from "./lib/Music.svelte";
    import Pomo from "./lib/Pomo.svelte";

    let date = new Date();
    let time = date.toLocaleTimeString("en-US", {
        hour: "2-digit",
        minute: "2-digit",
        hour12: true,
    });
    time = time.replace("AM", "").replace("PM", "");

    let day = date.getDate();

    const months = [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Sep",
        "Oct",
        "Nov",
        "Dec",
    ];
    let month = months[date.getMonth()];

    let weekdays = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
    ];
    let weekday = weekdays[date.getDay()];

    let formatDate = day + " " + month + " " + weekday;

    // feauture under dev alert
    function featureUnderDevAlert() {
        alert("This feature is currently development");
    }

    // music window
    let isMusicOpen = false;

    function openCloseMusic() {
        if (isMusicOpen) {
            isMusicOpen = false;
        } else {
            isMusicOpen = true;
        }
    }
</script>

<main>
    <div class="top">
        <div class="timebx">
            <div class="time">{time}</div>
            <div class="date">{formatDate}</div>
        </div>

        <div class="toolbar">
            <button class="tool" on:click={openCloseMusic}>
                <MusicNote weight="fill" size={20}></MusicNote>
            </button>
            <button
                class="tool"
                on:click={() => {
                    window.open("https://github.com/akshdzn/Muse", "_blank");
                }}
            >
                <GithubLogo weight="fill" size={20}></GithubLogo>
            </button>
        </div>
    </div>

    <div
        class={isMusicOpen
            ? "music-window-pos"
            : "music-window-pos music-hidden"}
    >
        <Music></Music>
    </div>

    <div class="bottom">
        <Pomo></Pomo>
    </div>
</main>
