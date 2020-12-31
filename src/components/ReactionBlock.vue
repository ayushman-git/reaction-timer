<template>
  <div v-if="showBlock" @click="stopReactionTimer">
    <h1 v-if="!timerStopped">Tap</h1>
    <div class="result" v-if="timerStopped">
      <h1>{{ reactionTime }}</h1>
      <h2>{{ resultText }}</h2>
    </div>
  </div>
</template>

<script>
export default {
  name: "ReacitonBlock",
  props: ["delay"],
  data() {
    return {
      showBlock: false,
      reactionTime: 0,
      interval: null,
      timerStopped: false,
    };
  },
  mounted() {
    console.log(this.delay);
    setTimeout(() => {
      this.showBlock = true;

      this.interval = setInterval(() => {
        this.reactionTime++;
        console.log(this.reactionTime);
      });
    }, this.delay);
  },
  computed: {
    resultText() {
      let result = "";
      if (this.reactionTime <= 50) {
        result = "Best in the World";
      } else if (this.reactionTime <= 100) {
        result = "Pro Gamer";
      } else if (this.reactionTime <= 200) {
        result = "A little better than average";
      } else if (this.reactionTime <= 300) {
        result = "Average";
      } else if (this.reactionTime <= 500) {
        result = ":/ Not that good";
      } else {
        result = "🐌";
      }
      return result;
    },
  },
  methods: {
    stopReactionTimer() {
      clearInterval(this.interval);
      this.timerStopped = true;
    },
  },
};
</script>

<style scoped>
div {
  width: 100%;
  height: 100vh;
  background-color: springgreen;
  display: flex;
  align-items: center;
  user-select: none;
  justify-content: center;
}
h1 {
  font-size: 7vw;
}
.result {
  display: flex;
  flex-direction: column;
}
</style>