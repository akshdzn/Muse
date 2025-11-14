<script>
    import { Link } from "phosphor-svelte";

    let urlInput;

    function getYouTubeVideoID(url) {
        const regex =
            /(?:https?:\/\/)?(?:www\.)?(?:youtube\.com\/(?:[^\/\n\s]+\/\S+\/|(?:v|e(?:mbed)?)\/|.*[?&]v=)|youtu\.be\/)([a-zA-Z0-9_-]{11})/;
        const matches = url.match(regex);
        return matches ? matches[1] : null;
    }
</script>

<div class="music-window">
    <div class="player"></div>
    <div class="music-input">
        <Link size={18} weight="regular"></Link>
        <input
            type="text"
            bind:value={urlInput}
            placeholder="Enter a YouTube Link"
        />
    </div>
</div>
