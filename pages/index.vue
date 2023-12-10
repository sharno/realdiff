<script setup>
import { diffLines } from "diff";

const str1 = ref("");
const str2 = ref("");

const diff = computed(() => {
  const d = diffLines(str1.value, str2.value);
  const lines = d.flatMap((block) =>
    block.value.split("\n").map((line) => ({ ...block, value: line }))
  );
  return lines;
});
</script>
<template>
  <h1 class="title">Real Diff</h1>

  <div class="columns py-4">
    <div class="column is-half">
      <textarea
        class="textarea"
        rows="6"
        v-model="str1"
        placeholder="Write text here"
      ></textarea>
    </div>

    <div class="column is-half">
      <textarea
        class="textarea"
        rows="6"
        v-model="str2"
        placeholder="Write text here"
      ></textarea>
    </div>
  </div>

  <p v-if="diff.length === 1 && diff[0].value === ''">Diff will appear here</p>

  <div>
    <div
      class="box"
      contenteditable="true"
      oncut="return false"
      onpaste="return false"
      onkeydown="if(event.metaKey) return true; return false;"
    >
      <div
        v-for="block in diff"
        :style="{
          backgroundColor: block.added
            ? '#80c492'
            : block.removed
            ? '#f77e7e'
            : 'white',
        }"
      >
        {{ block.value }}
      </div>
    </div>
  </div>
</template>
