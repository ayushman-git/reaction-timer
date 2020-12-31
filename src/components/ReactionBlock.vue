<template>
  <section>
    <div @click="preventSpam" v-if="!showBlock" class="prevent-spam"></div>
    <div class="stupid-div" v-if="showSpamMsg">
      <h1 class="spam-msg">You clicked too soon</h1>
    </div>
    <div class="reaction-wrapper" v-if="showBlock" @click="stopReactionTimer">
      <h1 v-if="!timerStopped">Tap</h1>
      <div class="result" v-if="timerStopped">
        <h1>{{ reactionTime }} ms</h1>
        <h2>{{ resultText }}</h2>
      </div>
    </div>
  </section>
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
      showSpamMsg: false,
      defaultInterval: null,
    };
  },
  mounted() {
    this.defaultInterval = setTimeout(() => {
      this.showBlock = true;

      this.interval = setInterval(() => {
        this.reactionTime++;
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
    preventSpam() {
      this.showSpamMsg = true;
      clearTimeout(this.defaultInterval);
      setTimeout(() => {
        this.$emit("stop");
      }, 2000);
    },
    stopReactionTimer() {
      if (this.timerStopped) {
        this.$emit("stop");
      }
      clearInterval(this.interval);
      this.timerStopped = true;
    },
  },
};
</script>

<style scoped>
section,
.stupid-div {
  width: 100%;
  height: 100vh;
}
.stupid-div {
  background-color: white;
  z-index: 5;
}
.prevent-spam {
  width: 100%;
  height: 100vh;
}
.reaction-wrapper {
  width: 100%;
  height: 100vh;
  background-color: springgreen;
  display: flex;
  align-items: center;
  user-select: none;
  justify-content: center;
  text-align: center;
}
h1 {
  font-size: 7vw;
}
.result {
  display: flex;
  flex-direction: column;
}
.spam-msg {
  color: red;
  position: absolute;
  top: 10%;
  left: 50%;
  transform: translateX(-50%);
  font-size: 3vw;
}
</style>