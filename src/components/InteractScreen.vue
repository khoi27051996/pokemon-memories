<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardsHandle.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsHandle.length)
        }px`,
      }"
    >
      <card-game-screen
        v-for="(card, index) in cardsHandle"
        :key="index"
        ref="myFunction"
        :imageBackFaceUrl="`images/${card}.png`"
        :cardCLick="{ index, value: card }"
        @onFlip="checkRules($event)"
        :cardsContext="cardsHandle"
      />
    </div>
  </div>
</template>

<script>
import CardGameScreen from "./CardGameScreen.vue";

export default {
  data() {
    return {
      rules: [],
    };
  },
  props: {
    cardsHandle: {
      type: [Array, Number, String, Object],
      defalut: function () {
        return [];
      },
    },
  },
  components: {
    CardGameScreen,
  },
  methods: {
    checkRules(card) {
      if (this.rules.length === 2) return false;

      this.rules.push(card);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        this.$refs.myFunction[this.rules[0].index].onDisabaleMode();
        this.$refs.myFunction[this.rules[1].index].onDisabaleMode();

        this.rules = [];

        const disabledElements = document.querySelectorAll(
          ".screen .card.isDisabled"
        );
        if (
          disabledElements &&
          disabledElements.length === this.cardsHandle.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 800);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        setTimeout(() => {
          this.$refs.myFunction[this.rules[0].index].onFlipBackCard();
          this.$refs.myFunction[this.rules[1].index].onFlipBackCard();
          this.rules = [];
        }, 800);
      } else return false;
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}

.screen__inner {
  width: calc(424px);
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>