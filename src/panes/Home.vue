<script setup>

import { ref } from 'vue'

const props = defineProps({
  title: String,
  count: Number,
})

const emit = defineEmits(['monarchUpdate', 'monarchReject']);
const stuff = ref('stuff');

function doClick () {
  setTimeout(() => {
    emit('monarchUpdate', stuff);
  }, 5000);
}

</script>

<template>
  <h1>{{ title }}</h1>
  <form>
    <input type="text" v-model="stuff" />
    <button type="button" @click="doClick">Increase Count</button>
    <button type="button" @click="this.$emit('monarchReject')">Reject!</button>
  </form>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>
