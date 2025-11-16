<script>
    import { Link } from "phosphor-svelte";
    import { onMount } from "svelte";

    let urlInput = "https://youtu.be/OkNo_N85em0?si=HdIGiRXSk4RTeX_6";
    let urlFinal;

    function getYouTubeVideoID(url) {
        const regex =
            /(?:https?:\/\/)?(?:www\.)?(?:youtube\.com\/(?:[^\/\n\s]+\/\S+\/|(?:v|e(?:mbed)?)\/|.*[?&]v=)|youtu\.be\/)([a-zA-Z0-9_-]{11})/;
        const matches = url.match(regex);
        return matches ? matches[1] : null;
    }

    $: if (urlInput !== "") {
        urlFinal =
            "https://www.youtube.com/embed/" +
            getYouTubeVideoID(urlInput) +
            "?autoplay=0&color=white&fs=0&loop=1&playlist=" +
            getYouTubeVideoID(urlInput) +
            "&rel=0";
    }
</script>

<div class="music-window">
    <div class="player">
        <iframe
            title="yt-player"
            width="331"
            height="197"
            src={urlFinal}
            frameborder="0"
            allow="accelerometer; encrypted-media; gyroscope; picture-in-picture; fullscreen"
        ></iframe>
    </div>
    <div class="music-input">
        <Link size={18} weight="regular"></Link>
        <input
            type="text"
            bind:value={urlInput}
            placeholder="Enter a YouTube Link"
        />
    </div>
</div>
