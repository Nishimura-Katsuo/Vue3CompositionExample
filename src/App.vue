<script setup>

import Home from './panes/Home.vue'
import YoutubeEmbed from './panes/YoutubeEmbed.vue';

import { ref, watch } from 'vue';

// Set up app state management
function encode (data) {
  return btoa(JSON.stringify(data));
}

function decode (str) {
  try {
    return JSON.parse(atob(str));
  }
  catch (e) {
    return null;
  }
}

const state = ref(decode(window.location.hash.slice(1)) || {});

function updateHash () {
  let newhash = '#' + encode(state.value);
  window.history.replaceState(null, '', window.location.href.split('#').shift() + newhash);
  window.location.hash = newhash;
}

watch(state, (updateHash(), updateHash), { deep: true });

window.addEventListener('hashchange', function () {
  state.value = decode(window.location.hash.slice(1)) || state.value;
});

state.value.count = state.value.count || 0;
state.value.route = state.value.route || 'home';

function updateCount (stuff) {
  state.value.count++;
  console.log('Stuff value:', stuff.value);
}

function rejectCount () {
  state.value.count = 0;
}

</script>

<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <div>
    <a href="javascript:;" @click="state.route = 'home'">Home</a> | <a href="javascript:;" @click="state.route = 'youtube'">Youtube Pane</a> | <a href="javascript:;" @click="state.route = 'invalid'">Invalid Pane</a>
  </div>
  <div>Count is: {{ state.count }}</div>
  <YoutubeEmbed v-if="state.route === 'youtube'" title="Otherworld" url="https://www.youtube.com/watch?v=mWYwmM23Sqs" :count="state.count" />
  <Home v-else-if="state.route === 'home'" title="Event Demo" @monarchUpdate="updateCount" @monarchReject="rejectCount" :count="state.count" />
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
