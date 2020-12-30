<template>
  <div v-if="!isPlaying" @click="start" class="play-button">
    <svg
      id="play-icon"
      enable-background="new 0 0 320.001 320.001"
      viewBox="0 0 320.001 320.001"
      xmlns="http://www.w3.org/2000/svg"
    >
      <path
        d="m295.84 146.049-256-144c-4.96-2.784-11.008-2.72-15.904.128-4.928 2.88-7.936 8.128-7.936 13.824v288c0 5.696 3.008 10.944 7.936 13.824 2.496 1.44 5.28 2.176 8.064 2.176 2.688 0 5.408-.672 7.84-2.048l256-144c5.024-2.848 8.16-8.16 8.16-13.952s-3.136-11.104-8.16-13.952z"
      />
    </svg>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      isPlaying: false,
      delay: null,
    };
  },
  components: {},
  methods: {
    start() {
      this.isPlaying = true;
      this.delay = 3000 + Math.random() * 5000;
    },
  },
};
</script>

<style>
.play-button {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 3em;
  height: 3em;
  outline: none;
  border: none;
}
#play-icon {
  fill: rgb(22, 109, 160);
}
</style>
