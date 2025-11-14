<script>
    import { MusicNote, GithubLogo } from "phosphor-svelte";

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
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday",
    ];
    let weekday = weekdays[date.getDay() - 1];

    let formatDate = day + " " + month + " " + weekday;

    // feauture under dev alert
    function featureUnderDevAlert() {
        alert("This feature is currently dev");
    }
</script>

<main>
    <div class="top">
        <div class="timebx">
            <div class="time">{time}</div>
            <div class="date">{formatDate}</div>
        </div>

        <div class="toolbar">
            <div class="tool" on:click={featureUnderDevAlert}>
                <MusicNote weight="fill" size={20}></MusicNote>
            </div>
            <div
                class="tool"
                on:click={window.open(
                    "https://github.com/akshdzn/Muse",
                    "_blank",
                )}
            >
                <GithubLogo weight="fill" size={20}></GithubLogo>
            </div>
        </div>
    </div>
</main>
