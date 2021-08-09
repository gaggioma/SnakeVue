<template>
  <div id="app">
    <Arena></Arena>
  </div>
</template>

<script>
import Arena from "./components/Arena";

export default {
  name: "App",

  components: {
    Arena,
  },
};
</script>

<style lang="scss">
#app {
  background-color: black;
  margin: -8px;
}
</style>
