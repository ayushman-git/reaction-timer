<template>
  <section @click="click">
    <h2>{{ header }}</h2>
    <p>{{ content }}</p>
  </section>
</template>

<script>
export default {
  name: "PlayCard",
  props: ["header", "content", "type"],
  methods: {
    click() {
      this.$emit("typeSelected", this.type);
    },
  },
};
</script>

<style scoped>
section {
  text-align: center;
  border-radius: 20px;
  background-color: rgb(255, 255, 255);
  padding: 1em 2em;
  user-select: none;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  transition: all 0.2s ease-in-out;
  cursor: pointer;
}

section:hover {
  box-shadow: 0px 0px 30px rgba(0, 0, 0, 0.2);
  color: rgb(163, 53, 53);
  background-color: black;
}

h2 {
  font-weight: 900;
  margin: 0.4em;
}

p {
  margin-top: 2em;
}
</style>