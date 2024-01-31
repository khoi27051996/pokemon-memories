<template>
  <main-screen
    v-if="isScreen === 'default'"
    @click="onChangeScreen"
    @onGame="onHandaleCard($event)"
  />
  <interact-screen
    v-if="isScreen === 'main'"
    :cardsHandle="cardsHandle"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="isScreen === 'result'"
    :timer="timer"
    @onReturnMain="returnMainScreen"
  />
  <coppy-right/>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { shuffer } from "./util/array";
import ResultScreen from "./components/ResultScreen.vue";
import CoppyRight from "./components/CoppyRight.vue";
export default {
  data() {
    return {
      isScreen: "default",
      totalOfBlock: 0,
      cardsHandle: [],
      starteAt: null,
      timer: 0,
    };
  },
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CoppyRight,
  },
  methods: {
    onChangeScreen() {
      this.starteAt = new Date().getTime();
      this.isScreen = "main";
      console.log(this.starteAt);
    },
    onHandaleCard(config) {
      this.totalOfBlock = config.totalOfBlock;

      const firstArr = Array.from(
        { length: this.totalOfBlock / 2 },
        (_, i) => i + 1
      );

      const secondArr = [...firstArr];

      const cardTotal = [...firstArr, ...secondArr];

      this.cardsHandle = shuffer(shuffer(shuffer(cardTotal)));
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.starteAt;
      this.isScreen = "result";
      console.log(this.timer);
    },
    returnMainScreen() {
      this.isScreen = "default";
    },
  },
};
</script>

<style>
</style>
