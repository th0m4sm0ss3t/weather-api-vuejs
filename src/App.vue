<template>
  <div class="wrapper">
    <Header />
    <Weather />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Weather from "./components/Weather.vue";

export default {
  name: "App",
  components: {
    Header,
    Weather,
  },
};
</script>

<style lang="scss">
@import "./scss/reset.scss";
@import "./scss/vars.scss";

#app {
  font-family: $textFont;
  font-size: $textSize;
}

.wrapper {
  background-color: $lightMint;
  min-height: 100vh;
  font-family: 'Dosis', sans-serif;
}
</style>
