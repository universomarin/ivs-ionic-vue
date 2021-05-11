<template>
  <div id="container">
    <strong>{{ name }}</strong>
    <video id="video-player"></video>   
  </div>
</template>


<script>
  import videojs from 'video.js';
  import { registerIVSTech } from 'amazon-ivs-player';
  import 'video.js/dist/video-js.css';


  export default {
    name: 'ExploreContainer',
    props: {
      name: String
    },
    data: () => ({
      player: null,
      videoSource: process.env.VUE_APP_CHAN_ENDPOINT,
      videoOptions: {
        autoplay: true,
        controls: true,
        techOrder: ["AmazonIVS"],
        width: "800"
      },
    }),

    mounted() {
      // register the tech with videojs
      console.log(`wasmWorker: ${this.createAbsolutePath('/assets/amazon-ivs-wasmworker.min.js')}`)

      registerIVSTech(videojs,  {
        wasmWorker: this.createAbsolutePath('/assets/amazon-ivs-wasmworker.min.js'),
        wasmBinary: this.createAbsolutePath('/assets/amazon-ivs-wasmworker.min.wasm'),
      });

      (IVSPlayerPackage) => {
          // First, check if the browser supports the IVS player.
          if (!IVSPlayerPackage.isPlayerSupported) {
              console.warn("The current browser does not support the IVS player.");
              return;
          }

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
          player.addEventListener(PlayerEventType.TEXT_METADATA_CUE, (cue) => {
              const metadataText = cue.text;
              const position = player.getPosition().toFixed(2);
              console.log(
                  `PlayerEvent - TEXT_METADATA_CUE: "${metadataText}". Observed ${position}s after playback started.`
              );
          });

          // Setup stream and play
          player.setAutoplay(true);
          player.src(this.videoSource);
          player.setVolume(0.5);
      }
      (window.IVSPlayer);
    },

    beforeUnmount() {
      // Destroy the player instance
      if(this.player) {
        this.player.dispose();
      }
    },

    methods: {
      createAbsolutePath(assetPath) {
        console.log( document.URL );
        return new URL(assetPath, document.URL).toString();
      },
    }
  }
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
</style>