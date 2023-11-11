<template>
  <div class="wrapper">
    <div></div>
  </div>
</template>
<script setup lang="ts">
import { ref } from "vue";

const questions = ref([
  {
    questions: "ถ้าวันนี้เป็นวันหยุด คุณจะ...",
  },
]);
</script>
<style scoped>
.wrapper {
  height: 478px;
  width: 316px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-shrink: 0;
  border-radius: 20px;
  border: 1px solid #000;
  background: #fff;
}
</style>
