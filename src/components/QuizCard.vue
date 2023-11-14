<template>
  <!-- Start Page -->
  <div v-if="showStartPage" class="start-page">
    <img :src="backgroundImage" alt="Background" class="background-image" />
    <button @click="startQuiz" class="start-button">เริ่มเลย</button>
  </div>

  <!-- Quiz -->
  <div
    v-else-if="!showStartPage && currentQuestionIndex < questions.length"
    class="wrapper"
  >
    <!-- Pagination Line -->
    <div class="pagination">
      <div
        v-for="n in questions.length"
        :key="n"
        class="pagination-item"
        :class="{ active: currentQuestionIndex === n - 1 }"
      ></div>
    </div>
    <div class="question-num">
      <h1>{{ "Q" + questions[currentQuestionIndex].id }}</h1>
    </div>
    <div v-if="currentQuestionIndex < questions.length" class="quiz">
      <h2 class="question">{{ questions[currentQuestionIndex].text }}</h2>
      <div class="choices">
        <div
          v-for="choice in questions[currentQuestionIndex].choices"
          :key="choice.score"
          class="choice"
        >
          <button class="btn" @click="selectChoice(choice.score)">
            {{ choice.text }}
          </button>
        </div>
      </div>
    </div>
    <div class="btn-page">
      <button
        @click="goBack"
        :disabled="currentQuestionIndex === 0"
        class="back"
      >
        กลับ
      </button>
      <button @click="goNext" class="next">
        {{
          currentQuestionIndex === questions.length - 1 ? "ดูผลลัพธ์" : "ต่อไป"
        }}
      </button>
    </div>
  </div>

  <!-- Display Result -->
  <div v-else class="results">
    <h2 class="text-result">ผลลัพธ์ของคุณคือ</h2>
    <img :src="personalityType" alt="Personality Type" class="img-result" />
    <button @click="resetQuiz">Retake Quiz</button>
  </div>
</template>

<script setup lang="ts">
import { ref, Ref, watch } from "vue";
import backgroundImage from "@/assets/bg.png";
import result1 from "@/assets/gradual.png";
import result2 from "@/assets/enthusiastic.png";
import result3 from "@/assets/hobbyist.png";

interface Choice {
  text: string;
  score: number;
}

interface Question {
  id: number;
  text: string;
  choices: Choice[];
}

const showStartPage: Ref<boolean> = ref(true);

const questions: Question[] = [
  {
    id: 1,
    text: "ถ้าวันนี้เป็นวันหยุด คุณจะ...",
    choices: [
      { text: "นอนเล่น พักผ่อน", score: 10 },
      { text: "ดูหนัง ดูซีรี่ส์ เล่นเกม", score: 20 },
      { text: "เรียนรู้อะไรใหม่ ๆ", score: 30 },
    ],
  },
  {
    id: 2,
    text: "คุณต้องการพัฒนาภาษาอังกฤษเพื่ออะไร?",
    choices: [
      { text: "การทำงาน", score: 30 },
      { text: "เที่ยวต่างประเทศ", score: 10 },
      { text: "ใช้ในชีวิตประจำวัน", score: 20 },
    ],
  },
  {
    id: 3,
    text: "สไตล์การเรียนภาษาอังกฤษของคุณเป็นแบบไหน?",
    choices: [
      { text: "เรียนคอร์สออนไลน์/โรงเรียนสอนภาษา", score: 30 },
      { text: "เรียนด้วยตนเอง เช่น อ่านหนังสือ ดูหนัง ฟังเพลง", score: 20 },
      { text: "คุยกับเพื่อนต่างชาติ/ไปเที่ยวต่างประเทศ", score: 10 },
    ],
  },
  {
    id: 4,
    text: "คุณใช้ภาษาอังกฤษบ่อยแค่ไหน?",
    choices: [
      { text: "เป็นประจำทุกวัน", score: 30 },
      { text: "เป็นบางครั้ง", score: 10 },
      { text: "นาน ๆ ที/ไม่ได้ใช้เลย", score: 20 },
    ],
  },
  {
    id: 5,
    text: "คุณอยากพัฒนาสกิลภาษาอังกฤษสกิลไหนมากที่สุด?",
    choices: [
      { text: "การฟัง", score: 20 },
      { text: "การพูด", score: 10 },
      { text: "การเขียน", score: 30 },
    ],
  },
  {
    id: 6,
    text: "อยากเรียนภาษาอังกฤษ แต่...",
    choices: [
      { text: "ไม่มีเวลา", score: 20 },
      { text: "เนื้อหาไม่น่าสนใจ/สอนไม่สนุก", score: 10 },
      { text: "ไม่มั่นใจ", score: 30 },
    ],
  },
  {
    id: 7,
    text: "คุณรู้จัก Globish ผ่านช่องทางใด (เลือกได้มากกว่า ๅ ข้อ)",
    choices: [
      { text: "Instagram", score: 0 },
      { text: "Facebook", score: 0 },
      { text: "อื่นๆ(โปรดระบุ)", score: 0 },
    ],
  },
];

const currentQuestionIndex: Ref<number> = ref(0);
const selectedAnswers: Ref<number[]> = ref<number[]>(
  Array(questions.length).fill(0)
);

enum PersonalityType {
  GradualLearner = result1,
  Enthusiastic = result2,
  Hobbyist = result3,
  Undefined = result3,
}

const startQuiz = (): void => {
  showStartPage.value = false;
};

const selectChoice = (score: number): void => {
  selectedAnswers.value[currentQuestionIndex.value] = score;
};

const calculateTotalScore = () => {
  // Sum up the scores from all answered questions
  return selectedAnswers.value.reduce((total, score) => total + score, 0);
};

const determinePersonalityType = (totalScore: number): PersonalityType => {
  if (totalScore >= 131 && totalScore <= 180)
    return PersonalityType.GradualLearner;
  if (totalScore >= 91 && totalScore <= 130)
    return PersonalityType.Enthusiastic;
  if (totalScore >= 60 && totalScore <= 90) return PersonalityType.Hobbyist;
  return PersonalityType.Undefined; // For scores outside the defined ranges
};

const personalityType: Ref<PersonalityType | string> = ref("");

watch(currentQuestionIndex, (newValue) => {
  // Check if the user has finished answering all questions
  if (newValue === questions.length) {
    const totalScore = calculateTotalScore();
    personalityType.value = determinePersonalityType(totalScore);
  }
});

const goBack = (): void => {
  if (currentQuestionIndex.value > 0) {
    currentQuestionIndex.value -= 1;
  }
};

const goNext = (): void => {
  // If it's the last question, calculate the results
  if (currentQuestionIndex.value === questions.length - 1) {
    const totalScore = calculateTotalScore();
    personalityType.value = determinePersonalityType(totalScore);
    currentQuestionIndex.value += 1; // Increment to show results
  } else {
    // For other questions, just go to the next question
    currentQuestionIndex.value += 1;
  }
};

const resetQuiz = (): void => {
  currentQuestionIndex.value = 0;
  selectedAnswers.value.fill(0);
  personalityType.value = "";
};
</script>

<style scoped>
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
}

.pagination-item {
  width: 50px;
  height: 8px;
  background-color: #cccccc;
  margin: 10px 4px -20px 4px;
  transition: background-color 0.3s;
  border-radius: 20px;
}

.pagination-item.active {
  background-color: #333; /* Active color */
}
.start-page {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
  position: relative;
}
.background-image {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: -1;
}
.start-button {
  width: 80%;
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
  z-index: 1;
  margin-top: 44rem;
}
.wrapper {
  height: 54%;
  width: 75%;
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
  color: #c8c8c8;
  font-size: 24px;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
}
h1 {
  font-size: 36px;
  font-family: "Gloria Hallelujah";
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
  padding: 10px 15px 0 30px;
  text-align: left;
}
.choices {
  margin-top: 40px;
}
.choice {
  margin-top: 15px;
}
.btn {
  background: #fff;
  color: #222;
  font-weight: 500;
  width: 80%;
  height: 72px;
  border: 1px solid #d9d9d9;
  text-align: left;
  border-radius: 20px;
  cursor: pointer;
  color: #10153c;
  font-size: 16px;
  font-style: normal;
  font-weight: 500;
  line-height: normal;
  padding-left: 26px;
  padding-right: 30px;
}
.btn:hover {
  background: #f57c4a;
  border: 1px solid #000;
}
.btn:focus {
  background: #f57c4a;
  border: 1px solid #000;
}
.btn-page {
  width: 100%;
  height: 20%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 90px;
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
.results {
  height: 70%;
  width: 80%;
  border-radius: 20px;
  border: 1px solid #000;
  background: #fff;
  color: var(--Gray-1, #333);
  text-align: center;
  font-size: 20px;
  font-style: normal;
  font-weight: 700;
  line-height: normal;
}
.text-result {
  color: var(--Gray-1, #333);
  text-align: center;
  font-size: 32px;
  font-style: normal;
  font-weight: 700;
  line-height: normal;
  padding-top: 2rem;
}
.img-result {
  width: 95%;
  height: 50%;
  flex-shrink: 0;
  margin-top: 1rem;
}
</style>
