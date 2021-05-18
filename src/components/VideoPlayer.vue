<template>
  <div id="container">
    <video id="video-player" playsinline></video>
  </div>
</template>

<script>

export default {
  name: "Player",
  props: {},

  mounted: function () {
    const IVSPlayerPackage = window.IVSPlayer;
    const PlayerState = IVSPlayerPackage.PlayerState;
    const PlayerEventType = IVSPlayerPackage.PlayerEventType;

    // Initialize player
    const player = IVSPlayerPackage.create();
    console.log("IVS Player version:", player.getVersion());
    player.attachHTMLVideoElement(document.getElementById("video-player"));

    // Attach event listeners
    player.addEventListener(PlayerState.PLAYING, function () {
      console.log("Player State - PLAYING");
    });
    player.addEventListener(PlayerState.ENDED, function () {
      console.log("Player State - ENDED");
    });
    player.addEventListener(PlayerState.READY, function () {
      console.log("Player State - READY");
    });
    player.addEventListener(PlayerEventType.ERROR, function (err) {
      console.warn("Player Event - ERROR:", err);
    });

    // Setup stream and play
    player.setAutoplay(true);
    player.load(
      "https://fcc3ddae59ed.us-west-2.playback.live-video.net/api/video/v1/us-west-2.893648527354.channel.DmumNckWFTqz.m3u8"
    );
    player.setVolume(0.0);
  }
};
</script>

<style scoped>
#container {
  display: flex;
  justify-content: center;
  align-content: center;
  text-align: center;
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#video-player {
  width: 90%;
}
</style>