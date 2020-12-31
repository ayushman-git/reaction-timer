<template>
  <section v-if="showBlock">
    <div
      v-if="timeLeft > 0"
      @click="blobClickHandler"
      class="blob-container"
      ref="container"
    ></div>
    <div v-if="timeLeft <= 0">
      <header>
        <p>{{ successfulClicks }}</p>
        <p>{{ failedClicks }}</p>
      </header>
      <h1>{{ accuracyRate }}</h1>
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
      delay: 200,
    };
  },
  mounted() {
    this.randomBlobGen();
    setTimeout(() => {
      clearInterval(this.blobInterval);
      console.log(this.successfulClicks, this.failedClicks);
    }, 1000 * 30);
  },
  computed: {
    accuracyRate() {
      const result =
        (this.successfulClicks * 100) / this.successfulClicks +
        this.failedClicks;
      return result;
    },
  },
  methods: {
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

      this.blobInterval = setInterval(() => {
        this.delay -= 50;
        console.log(this.delay);
        this.decrementTimeLeft();
        const blob = document.createElement("div");
        blob.classList.add("blob");
        blob.style.position = "absolute";
        blob.style.width = "30px";
        blob.style.height = "30px";
        blob.style.backgroundColor = "yellow";
        blob.style.borderRadius = "50%";
        blob.style.left = Math.floor(Math.random() * containerWidth) + "px";
        blob.style.top = Math.floor(Math.random() * containerHeight) + "px";
        container.appendChild(blob);
      }, this.delay);
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
</style>