<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="finishGame()"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :playTime="playTime"
    @onStartAgain="statusMatch = 'default'"
  />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";

export default {
  Name: "App",

  data() {
    return {
      settings: {
        maxCards: 64,
        totalOfBlock: 0,
        cardsContext: [],
        startedAt: null,
        playTime: 0,
      },
      statusMatch: "default",
    };
  },
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlock = config.totalOfBlocks;
      const arrayCard = Array.from(
        { length: this.settings.totalOfBlock / 2 },
        () => Math.floor(Math.random() * this.settings.maxCards) + 1
      );
      const allCard = [...arrayCard, ...arrayCard];
      this.settings.cardsContext = allCard.sort(() => Math.random() - 0.5);
      this.settings.startedAt = new Date().getTime();
      this.statusMatch = "match";
    },
    finishGame() {
      //get timmer
      this.playTime = new Date().getTime() - this.settings.startedAt;
      console.log(this.playTime);
      this.statusMatch = "result";
    },
  },
};
</script>
