<template>
  <div>
    <main-screen 
      v-if="statusMatch === 'default'" 
      @onStart="onHandleBeforeStart($event)" />
    <interact-screen 
      v-if="statusMatch === 'match'" 
      :cardsContext="settings.cardsContext"
      @onFinish="onGetResult()"
    />
    <result-screen 
      v-if="statusMatch === 'result'" 
      :timer="timer"
      @onStartAgain="statusMatch = 'default'"
    />
    <copy-right />
  </div>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { shuffled } from "./utils/array";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRight from "./components/CopyRight.vue";

export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight,
  },
  data() {
    return {
      statusMatch: "default",
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      timer: 0,
    }
  },
  
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCards = Array.from({ length: this.settings.totalOfBlocks / 2 }, (_, i) => i + 1);

      const secondCards = [...firstCards];

      const cards = [...firstCards, ...secondCards];

      this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))));

      this.settings.startedAt = new Date().getTime();

      this.statusMatch = "match";
    },

    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startedAt;

      this.statusMatch = "result";
    }
  }
};
</script>
