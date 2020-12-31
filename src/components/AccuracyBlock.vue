<template>
  <section v-if="showBlock">
    <div
      v-if="timeLeft > 0"
      @click="blobClickHandler"
      class="blob-container"
      ref="container"
    ></div>
    <div class="result" v-if="timeLeft <= 0" @click="stopPlaying">
      <header>
        <p>Successful Clicks - {{ successfulClicks }}</p>
        <p>Missed Clicks - {{ failedClicks }}</p>
      </header>
      <h1>{{ accuracyRate }}%</h1>
    </div>
    <h2
      class="time-left"
      :style="[timeLeft < 10 ? { color: 'red' } : {}]"
      v-if="timeLeft > 0"
    >
      {{ timeLeft }}
    </h2>
  </section>
</template>

<script>
export default {
  name: "AccuracyBlock",
  data() {
    return {
      showBlock: true,
      blobInterval: null,
      successfulClicks: 0,
      failedClicks: 0,
      timeLeft: 30,
      delay: 400,
      timeInterval: null,
      readyToExit: false,
    };
  },
  mounted() {
    this.randomBlobGen();
    setTimeout(() => {
      clearInterval(this.blobInterval);
      clearInterval(this.timeInterval);
      clearInterval(this.blobDelInterval);
      setTimeout(() => {
        this.readyToExit = true;
      }, 2000);
    }, 1000 * 30);
  },
  computed: {
    accuracyRate() {
      const result =
        this.successfulClicks *
        (100 / (this.successfulClicks + this.failedClicks));
      return result.toFixed(2);
    },
  },
  methods: {
    stopPlaying() {
      if (this.readyToExit) {
        this.$emit("stop");
      }
    },
    blobClickHandler(e) {
      if (e.target.classList.value === "blob") {
        this.successfulClicks++;
        e.target.remove();
      } else {
        this.failedClicks++;
      }
    },
    decrementTimeLeft() {
      this.timeLeft--;
    },
    randomBlobGen() {
      const { container } = this.$refs;
      const containerWidth = container.offsetWidth;
      const containerHeight = container.offsetHeight;

      this.timeInterval = setInterval(() => {
        this.decrementTimeLeft();
      }, 1000);
      let counter = 0;
      let delCounter = 0;
      this.blobInterval = setInterval(() => {
        counter++;
        const blob = document.createElement("div");
        blob.classList.add("blob");
        blob.setAttribute("id", "blob-" + counter);
        blob.style.position = "absolute";
        blob.style.width = "30px";
        blob.style.height = "30px";
        blob.style.backgroundColor = "rgb(59,68,131)";
        blob.style.borderRadius = "50%";
        blob.style.left = Math.floor(Math.random() * containerWidth) + "px";
        blob.style.top = Math.floor(Math.random() * containerHeight) + "px";
        container.appendChild(blob);
      }, this.delay);
      setTimeout(() => {
        this.blobDelInterval = setInterval(() => {
          delCounter++;
          if (document.getElementById("blob-" + delCounter)) {
            document.getElementById("blob-" + delCounter).remove();
          }
        }, 500);
      }, 5000);
    },
  },
};
</script>

<style scoped>
section {
  width: 100%;
  height: 100vh;
  background: linear-gradient(#2c2c2c, #1a1a1a);
  display: flex;
  flex-flow: column nowrap;
  justify-content: center;
  align-items: center;
}
.blob-container {
  width: 60%;
  height: 70vh;
  position: relative;
}
.blob {
  position: absolute;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background-color: yellow;
}
.time-left {
  color: white;
}
.result {
  color: white;
  text-align: center;
  user-select: none;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.result h1 {
  font-size: 6vw;
}
header {
  display: flex;
}

header > :first-child {
  margin-right: 2em;
}
</style>