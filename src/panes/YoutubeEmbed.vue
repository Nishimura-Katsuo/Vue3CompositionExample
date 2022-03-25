<script setup>

import { ref, onMounted } from 'vue';

let player;

const playing = ref(false), currentTime = ref(0);

const props = defineProps({
  title: String,
  count: Number,
  visible: Boolean,
});

window.isYTReady = new Promise(resolve => {
  window.onYouTubeIframeAPIReady = resolve;
});

setInterval(() => {
  if (player && player.getCurrentTime) {
    currentTime.value = player.getCurrentTime();
  }
}, 40);

onMounted(() => {
  window.isYTReady.then(() => {
    player = new YT.Player('player', {
      width: '',
      height: '',
      videoId: '9Q3_fB1LqV0',
      playerVars: {
        'playsinline': 1
      },
      events: {
        'onStateChange': function (event) {
          playing.value = event.data === 1;
        },
      }
    });

    console.log(player);
  });
});


</script>

<template>
  <template v-if="visible">
    <h1>{{ title }}</h1>
    <p>The homepage count is: {{ count }}</p>
  </template>
  <div :class="{
    'youtube-frame': true,
    'youtube-frame-visible': visible,
    'youtube-frame-popout': (playing || currentTime) && !visible,
    'youtube-frame-hidden': !(playing || currentTime) && !visible,
  }">
    <div id="player"></div>
  </div>
</template>

<style>

.youtube-frame-visible {
  aspect-ratio: 16 / 9;
}

.youtube-frame-popout {
  position: fixed;
  right: 15px;
  bottom: 15px;
  width: 480px;
  aspect-ratio: 16 / 9;
}

.youtube-frame-hidden {
  display: none;
}

.youtube-frame > * {
  width: 100% !important;
  height: 100% !important;
}

</style>
