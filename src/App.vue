<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="this.timer"
    @onStartAgain="statusMatch = 'default'"
  />
  <copy-right-screen />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRightScreen from "./components/CopyRightScreen.vue";
import { shuffled } from "./utils/array";
export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRightScreen,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startAt: 0,
        timer: 0,
      },
      statusMatch: "default",
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log(config);
      this.settings.totalOfBlocks = config.totalOfBlocks;
      const firstCards = Array.from(
        {
          length: this.settings.totalOfBlocks / 2,
        },
        (_, i) => i + 1
      );
      const secondCard = [...firstCards];
      const cards = [...firstCards, ...secondCard];
      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      this.settings.startAt = new Date().getTime();
      console.log(this.settings.cardsContext);
      this.statusMatch = "match";
    },
    onGetResult() {
      // Get timmer
      this.timer = new Date().getTime() - this.settings.startAt;
      // change status match
      this.statusMatch = "result";
    },
  },
};
</script>
