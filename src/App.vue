<template>
  <div class="container">
    <QuizCard />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import QuizCard from "./components/QuizCard.vue";

export default defineComponent({
  name: "App",
  data() {
    return {
      firstName: "Anurat",
      lastName: "Likitdachavong",
      age: 21,
    };
  },
  components: {
    QuizCard,
  },
});
</script>

<style lang="scss">
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
