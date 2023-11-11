<template>
  <div class="wrapper">
    <div class="question-num">
      <img src="../assets/Q1.png" alt="question number" />
    </div>
    <div class="quiz">
      <h2 class="question">ถ้าวันนี้เป็นวันหยุด คุณจะ...</h2>
      <div id="answer-bottons">
        <button class="btn">นอนเล่น พักผ่อน</button>
        <button class="btn">ดูหนัง ดูซีรี่ส์ เล่นเกม</button>
        <button class="btn">เรียนรู้อะไรใหม่ ๆ</button>
      </div>
    </div>
    <div class="btn-page">
      <button class="back">กลับ</button>
      <button class="next">ต่อไป</button>
    </div>
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
  height: 51%;
  width: 40%;
  flex-shrink: 0;
  border-radius: 20px;
  border: 1px solid #000;
  background: #fff;
}
.question-num {
  display: flex;
  justify-content: start;
  align-items: start;
  padding: 35px 0 0 30px;
}
img {
  width: 40px;
  height: 35px;
  flex-shrink: 0;
}
.question {
  display: flex;
  justify-content: start;
  align-items: start;
  font-size: 16px;
  font-style: normal;
  font-weight: 700;
  line-height: normal;
  color: #000;
  padding: 30px 15px 0 30px;
  text-align: left;
}
#answer-bottons {
  margin-top: 40px;
}
.btn {
  background: #fff;
  color: #222;
  font-weight: 500;
  width: 80%;
  height: 72px;
  border: 1px solid #d9d9d9;
  padding: 10px;
  margin: 10px 0;
  text-align: left;
  border-radius: 20px;
  cursor: pointer;
  color: #10153c;
  font-family: IBM Plex Sans Thai;
  font-size: 16px;
  font-style: normal;
  font-weight: 500;
  line-height: normal;
  padding-left: 26px;
}
.btn:hover {
  background: #f57c4a;
  border: 1px solid #000;
}
.btn-page {
  width: 100%;
  height: 50%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 30px;
}
.back {
  width: 30%;
  height: 40px;
  border-radius: 20px;
  border: 1px solid #000;
  background: #fffaf5;
  color: #000;
  font-size: 20px;
  font-style: normal;
  font-weight: 500;
  line-height: normal;
  cursor: pointer;
}
.next {
  width: 60%;
  height: 40px;
  border-radius: 20px;
  border: 1px solid #000;
  background: #f7c116;
  color: #000;
  font-size: 20px;
  font-style: normal;
  font-weight: 500;
  line-height: normal;
  cursor: pointer;
}
</style>
