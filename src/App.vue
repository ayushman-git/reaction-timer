<template>
  <div
    id="backgroud"
    :style="{ background: 'linear-gradient(#FFFFFF, #ECE9E6)' }"
  >
    <h1 v-if="!isPlaying" class="main-heading">Test Your Skills</h1>
    <Info class="info" v-if="showInfo" :infoMsg="infoMsg" />
    <ReactionBlock
      @stop="stopHandler"
      v-if="isPlaying && typeSelected === 'reaction'"
      class="reaction-block"
      :delay="delay"
    />

    <AccuracyBlock @stop="stopHandler"  v-if="isPlaying && typeSelected === 'accuracy'" />

    <div v-if="!isPlaying" class="play-card-wrapper">
      <PlayCard
        @typeSelected="playHandler"
        header="Reaction Timer"
        content="Check your reaction time"
        type="reaction"
      />
      <PlayCard
        @typeSelected="playHandler"
        header="Accuracy Tester"
        content="Check your click accuracy"
        type="accuracy"
      />
    </div>
  </div>
</template>

<script>
import PlayCard from "./components/PlayCard";
import Info from "./components/Info";
import ReactionBlock from "./components/ReactionBlock";
import AccuracyBlock from "./components/AccuracyBlock";
export default {
  name: "App",
  components: { PlayCard, Info, ReactionBlock, AccuracyBlock },
  data() {
    return {
      isPlaying: false,
      delay: null,
      typeSelected: null,
      showInfo: false,
      infoMsg: "",
    };
  },
  methods: {
    stopHandler() {
      this.isPlaying = false;
    },
    playHandler(type) {
      this.isPlaying = true;
      this.delay = 5000 + Math.random() * 5000;
      this.typeSelected = type;
      if (this.typeSelected === "reaction") {
        this.infoMsg = "Tap anywhere as soon as color changes";
      }
      else {
        this.infoMsg = "Tap dots as soon as they appear";
      }
      this.showInfo = true;
      setTimeout(() => {
        this.showInfo = false;
      }, 2000);
    },
  },
};
</script>

<style>
#backgroud {
  width: 100%;
  height: 100vh;
  margin: 0;
  padding: 0;
}
.play-card-wrapper {
  position: absolute;
  display: flex;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -20%);
}

.play-card-wrapper > * {
  margin: 2em;
}

.info {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100%;
  padding: 2em;
}
.reaction-block {
  position: absolute;
}
.main-heading {
  font-size: 5vw;
  margin-top: 2em;
  text-align: center;
}
</style>
