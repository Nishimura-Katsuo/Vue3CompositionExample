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
  <div class="container">
    <div class="card my-2">
      <div class="card-header text-center">
        <a href="javascript:;" @click="state.route = 'home'">Home</a> | <a href="javascript:;" @click="state.route = 'youtube'">Youtube Pane</a> | <a href="javascript:;" @click="state.route = 'invalid'">Invalid Pane</a>
        <div>Count is: {{ state.count }}</div>
      </div>
      <div class="card-body">
        <YoutubeEmbed :visible="state.route === 'youtube'" title="Final Fantasy Guitar Collection" :count="state.count" />
        <Home v-if="state.route === 'home'" title="Event Demo" @monarchUpdate="updateCount" @monarchReject="rejectCount" :count="state.count" />
        <div v-if="!['youtube', 'home'].includes(state.route)">
          <h1>Invalid Pane</h1>
          <p>Page not found.</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style>

</style>
