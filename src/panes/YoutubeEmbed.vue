<script setup>

import { computed } from 'vue';

const props = defineProps({
  title: String,
  url: String,
  count: Number,
})

const embedUrl = computed(() => {
  let url = props.url.value || props.url;
  return 'https://www.youtube.com/embed/' + url.slice(32);
});

</script>

<template>
  <h1>{{ title || 'No Title' }}</h1>
  <div>
    <div class="youtube-iframe-wrapper">
      <p>The homepage count is: {{ count }}.</p>
      <iframe class="youtube-iframe" width="1368" height="769" :src="embedUrl" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    </div>
  </div>
</template>

<style scoped>
</style>
