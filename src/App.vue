<script setup>
import { marked } from "marked";
import { debounce } from "lodash-es";
import { ref, computed, nextTick } from "vue";
import example from "./assets/example.md?raw";

const input = ref(localStorage.getItem("input") || example);

const output = computed(() => marked(input.value));

const update = debounce((e) => {
  input.value = e.target.value;
  localStorage.setItem("input", e.target.value);
}, 100);

const reset = () => {
  input.value = example;
  localStorage.setItem("input", input.value);
};

const isPrinting = ref(false);

const print = () => {
  isPrinting.value = true;
  nextTick(() => {
    window.print();
    isPrinting.value = false;
  });
};
</script>


<template>
  <div class="h-screen flex w-full" v-if="!isPrinting">
    <textarea
      class="overflow-auto box-border w-5/12 h-full p-5 border-r-2 border-r-slate-300 resize-none outline-none bg-slate-100"
      spellcheck="false"
      :value="input"
      @input="update"
    >
    </textarea>
    <div
      class="overflow-auto flex-1 h-full py-5 px-10"
      v-html="output"
      id="markdown"
    ></div>
    <div class="fixed right-5 top-1 gap-2 flex">
      <button
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-1 px-2 rounded"
        @click="reset"
      >
        重置
      </button>
      <button
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-1 px-2 rounded"
        @click="print"
      >
        导出
      </button>
    </div>
  </div>
  <div
    v-else
    class="overflow-auto flex-1 h-full py-5 px-10"
    v-html="output"
    id="markdown"
  ></div>
</template>

<style>
@import url("./markdown.css");
</style>