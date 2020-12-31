<template>
  <h1>{{ infoMsg }}</h1>
</template>

<script>
export default {
  name: "Info",
  props: ["infoMsg"],
};
</script>

<style scoped>
h1 {
  text-align: center;
  font-size: 4vw;
  user-select: none;
}
</style>