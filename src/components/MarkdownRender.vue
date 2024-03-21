<template>
  <div
    class="overflow-auto flex-1 h-full py-5 px-10"
    v-html="output"
    id="markdown"
  ></div>
</template>

<script setup>
const props = defineProps({
  output: String,
});
</script>

<style>
@import url("../assets/markdown.css");
</style>