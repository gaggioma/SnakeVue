<template>
    <div :key="counter" class="containerCounter">
        <h1>
            {{ counter }}
        </h1>
    </div>
</template>

<script>
export default {
  created: function () {
    setInterval(() => this.decreaseCounter(), 2000);
  },

  data: () => {
    return {
      counter: 3,
    };
  },

  methods: {
    decreaseCounter() {
      this.counter = this.counter - 1;
      if (this.counter === 0) {
        this.$emit("zeroCounter");
      }
    },
  },
};
</script>

<style>

.containerCounter{
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  margin-left: 43%;
  margin-top: 10%;
  width: 200px;
  height: 200px;
  background: darkturquoise;
  animation: fadeinout 2s linear forwards;
  z-index: 1;
  border-radius: 200px;
}

@keyframes fadeinout {
  0%,
  100% {
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
}
</style>
