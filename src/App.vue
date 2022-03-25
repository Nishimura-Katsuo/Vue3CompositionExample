<script setup>

import Home from './panes/Home.vue'
import YoutubeEmbed from './panes/YoutubeEmbed.vue';

import { ref } from 'vue';

const route = ref(window.location.hash.slice(1)),
  count = ref(0);

window.addEventListener('hashchange', () => {
  route.value = window.location.hash.slice(1);
});

function updateCount (stuff) {
  count.value++;
  console.log('Stuff value:', stuff.value);
}

function rejectCount () {
  count.value = 0;
}

</script>

<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <div>
    <a href="#home">Home</a> | <a href="#youtube">Youtube Pane</a> | <a href="#invalid">Invalid Pane</a>
  </div>
  <div>Count is: {{ count }}</div>
  <YoutubeEmbed v-if="route === 'youtube'" title="Otherworld" url="https://www.youtube.com/watch?v=mWYwmM23Sqs" :count="count" />
  <Home v-else-if="route === 'home'" title="Event Demo" @monarchUpdate="updateCount" @monarchReject="rejectCount" :count="count" />
  <div v-else>
    <h1>Page Not Found</h1>
    <p>This is an invalid route.</p>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
