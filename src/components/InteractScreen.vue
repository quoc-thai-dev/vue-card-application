<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <!-- <button @click="this.$emit('onFinish')">Give up?</button> -->
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        :cardsContext="cardsContext"
        @onFlip="checkRule($event)"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./CardFlip.vue";

export default {
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
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length == 2) return false;
      this.rules.push(card);

      if (
        this.rules.length == 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        console.log("right");
        // add Class Disabled to component CardFlip
        // Reset rule to []
        // setTimeout(() => {
        this.$refs[`card-${this.rules[0].index}`][0].onDisabled();
        this.$refs[`card-${this.rules[1].index}`][0].onDisabled();
        this.rules = [];
        const disabledEle = document.querySelectorAll(".screen .card.disabled");
        if (disabledEle.length == this.cardsContext.length - 2) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 1000);
        }
        // }, 800);
      } else if (
        this.rules.length == 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        console.log("Wrongg...", this.rules);
        // close two card and rules to []
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onToggleFlipCard();
          this.$refs[`card-${this.rules[1].index}`][0].onToggleFlipCard();
          this.rules = [];
        }, 800);
      } else {
        return false;
      }
    },
  },
};
</script>
<style lang="css" scoped>
.screen {
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}
.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
