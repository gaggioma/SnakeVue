<template>
  <div class="levelComplete">
    <div class="insideClassText">👍 level complete in {{ time }} ms!!</div>
    <button class="buttonClass" @click="clickHandler($event)">Restart</button>
  </div>
</template>

<script>
export default {
  props: ["time"],

  methods:{

    clickHandler(){
      this.$emit("restart");
    }
  }

};
</script>

<style>
.levelComplete {
  position: fixed;
  font-size: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: black;
  z-index: 1;
  height: 100%;
  width: 100%;
  text-shadow: 0 0 3px azure;
  flex-direction: column;
}

.insideClassText {
  color: white;
  animation: pulse 1s infinite;
  transform: scale(1);
}

@keyframes pulse {
  0% {
    transform: scale(0.9);
  }

  70% {
    transform: scale(1);
  }

  100% {
    transform: scale(0.9);
  }
}

.buttonClass{
  margin-top: 5px;
  border-radius: 10px;
  height: 31px;
  background-color: red;
  color: white;
}

</style>
