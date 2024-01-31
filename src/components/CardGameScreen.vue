<template>
  <div
    class="card"
    :class="{ isDisabled: isDisabled }"
    :style="{
      height: `${(920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
      width: `${
        (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4
      }px`,
      perspective: `${
        ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4) * 2
      }px`,
    }"
  >
    <div
      class="card__container"
      :class="{ isFlip: isFlip }"
      @click="onFlipCard"
    >
      <div class="card__face card__face-front">
        <div
          class="card__content"
          :style="{
            backgroundSize: `${
              (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face-back">
        <div
          class="card__content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imageBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imageBackFaceUrl: {
      type: String,
      required: true,
    },
    cardCLick: {
      type: [String, Array, Number, Object],
    },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isFlip: false,
      isDisabled: false,
    };
  },
  methods: {
    onFlipCard() {
      if (this.isDisabled) return false;
      this.isFlip = !this.isFlip;
      if (this.isFlip) this.$emit("onFlip", this.cardCLick);
    },
    onFlipBackCard() {
      this.isFlip = false;
    },
    onDisabaleMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}
.card__container {
  width: 100%;
  height: 100%;
  position: relative;
  transition: all 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  border-radius: 10px;
  box-shadow: 0 1px 5px 3px rgba(0, 0, 0, 0.2);
}

.card.isDisabled .card__container {
  cursor: default;
}

.card__container.isFlip {
  transform: rotateY(-180deg);
}
.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  padding: 1rem;
  border-radius: 10px;
}

.card__face.card__face-front {
  background-color: var(--dark);
  width: 100%;
  height: 100%;
}

.card__face.card__face-front .card__content {
  background: url("../assets/images/icon_back.png") center center no-repeat;

  width: 100%;
  height: 100%;
}

.card__face.card__face-back {
  transform: rotateY(-180deg);
  background-color: var(--light) !important;
  width: 100%;
  height: 100%;
}
.card__face.card__face-back .card__content {
  height: 100%;
  width: 100%;
  background-position: center;
  background-repeat: no-repeat;
  background-size: contain;
}
</style>