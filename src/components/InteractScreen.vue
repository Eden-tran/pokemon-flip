<template>
  <div
    class="screen"
    :style="{
      height: '100%',
    }"
  >
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((100 - 2 * 2) / Math.sqrt(cardsContext.length)) * 2) / 3) *
          Math.sqrt(cardsContext.length)
        }%`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :cardsContext="cardsContext"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        :rules="rules"
        @onFlip="
          checkRule($event);
          checkWin();
        "
        :cardContext="cardContext"
        @onClose="checkClose($event)"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./Card.vue";
export default {
  data() {
    return {
      point: 0,
      rules: [],
    };
  },
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip,
  },
  methods: {
    checkWin() {
      if (this.point === this.cardsContext.length / 2) {
        setTimeout(() => {
          this.$emit("onFinish");
        }, 2000);
      } else {
        // console.log(this.point);
      }
    },
    checkRule(card) {
      console.log(this.rules);
      this.rules.push(card);
      if (this.rules.length > 2) {
        // this.rules = [];

        return false;
      }
      if (this.rules.length === 2) {
        if (this.rules[0].value === this.rules[1].value) {
          this.$refs[`card-${this.rules[0].index}`][0].onDisableMode();
          this.$refs[`card-${this.rules[1].index}`][0].onDisableMode();
          this.point++;
          this.rules = [];
        } else {
          // close 2 card
          setTimeout(() => {
            let range = this.rules.length - 1;
            for (let index = 0; index <= range; index++) {
              this.$refs[`card-${this.rules[index].index}`][0].onFlipBackCard();
            }
            this.rules = [];
          }, 800);
        }
      }
    },
    checkClose(card) {
      // if (this.rules.includes(card)) {
      //   re
      // }
      const index = card.index;
      const hasCard = this.rules.some((element) => element.index === index);
      if (hasCard) {
        this.rules = this.rules.filter((element) => element.index !== index);
      }
    },
  },
};
</script>
<style scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
}
.screen__inner {
  justify-content: center;
  height: 80%;
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
