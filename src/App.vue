<template>
  <div id="app">
    <Arena
    :key="keyVal"
    @restartArena="restartApp()"></Arena>
  </div>
</template>

<script>
import Arena from "./components/Arena";

export default {
  name: "App",

  components: {
    Arena,
  },

  data: function(){
    return{
      keyVal: Math.random()
    }
  },

  methods:{
    restartApp(){
      this.keyVal = Math.random();
    }
  }
};
</script>

<style lang="scss">
#app {
  background-color: black;
  margin: -8px;
}
</style>
