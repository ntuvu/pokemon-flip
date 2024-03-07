<template>
  <div
    class="card"
    :class="{ disable: isDisable }"
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
      class="card__inner"
      @click="onToggleFlipCard()"
      :class="{ 'is-flipped': isFlipped }"
    >
      <div class="card__face card__face--front">
        <div class="card__content"></div>
      </div>
      <div class="card__face card__face--back">
        <div class="card__content" :style="backFaceStyle"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    card: {
      type: [String, Number, Array, Object],
      required: true,
    },
    cardsContext: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      isFlipped: false,
      isDisable: false,
    };
  },
  computed: {
    backFaceStyle() {
      const imageUrl = new URL(
        `../assets/${this.imgBackFaceUrl}`,
        import.meta.url
      ).href;
      return {
        backgroundImage: `url('${imageUrl}')`,
      };
    },
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisable) {
        return false;
      }
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) {
        this.$emit("onFlip", this.card);
      }
    },

    onFlipBackCard() {
      this.isFlipped = false;
    },

    onEnableDisableMode() {
      this.isDisable = true;
    },
  },
};
</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
  width: 90px;
  height: 120px;
  caret-color: transparent;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 0.6s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card.disable .card__inner {
  cursor: not-allowed;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card__face--front .card__content {
  background: url("../assets//images//icon_back.png") no-repeat center center;
  background-size: 60px 60px;
  height: 100%;
  width: 100%;
}

.card__face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card__face--back .card__content {
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
}
</style>
