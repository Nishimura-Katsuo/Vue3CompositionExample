<script setup>

import { ref } from 'vue'

const props = defineProps({
  title: String,
  count: Number,
})

const emit = defineEmits(['monarchUpdate', 'monarchReject']);
const stuff = ref('stuff');

function doClick () {
  // You can pass stuff directly and give the parent a reactive reference, or
  // you can snapshot the value here with 'let stuffCapture = stuff.value;' to
  // capture the value at the time of the click.
  setTimeout(() => {
    emit('monarchUpdate', stuff);
  }, 2000);
}

</script>

<template>
  <h1>{{ title }}</h1>
  <form>
    <div class="row gx-2">
      <div class="col"><input class="form-control" type="text" v-model="stuff"></div>
      <div class="col-auto"><button class="btn btn-primary" type="button" @click="doClick">Increase Count</button></div>
      <div class="col-auto"><button class="btn btn-primary" type="button" @click="this.$emit('monarchReject')">Reject!</button></div>
    </div>
  </form>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>
