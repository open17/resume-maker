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
};

const print = () => {
  localStorage.setItem("input", input.value);
  let contentToPrint = document.getElementById("markdown");
  let originalContents = document.body.innerHTML;
  document.body.innerHTML = contentToPrint.innerHTML;
  window.print();
  nextTick(() => {
    document.body.innerHTML = originalContents;
    input.value = localStorage.getItem("input");
    location.reload();
  });
};
</script>


<template>
  <div class="h-screen flex w-full">
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
</template>

<style>
p {
  font-size: 0.9rem;
}
blockquote {
  display: flex;
  justify-content: center;
  align-items: center;
}
blockquote * {
  font-size: 0.8rem;
  text-align: center;
}
hr {
  border-top: 1px dashed #dcdcdd;
  margin-top: 0.5rem;
  margin-bottom: 0.5rem;
}
li {
  list-style: disc;
  margin-left: 1.25rem;
  margin-bottom: 0.5rem;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-weight: 600;
  line-height: 1.25;
  margin-bottom: 1.25rem;
  margin-top: 1rem;
}
h1 {
  font-size: 2.5rem;
}
h2 {
  display: flex;
  align-items: center;
  font-size: 1.25rem;
}
h2::after {
  content: "";
  flex: 1;
  border-bottom: 2px dashed #d3d3d3;
  margin-left: 10px;
}
h3 {
  font-size: 1rem;
}
em {
  float: right;
}
</style>