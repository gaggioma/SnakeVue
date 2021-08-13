<template>
  <div :key="counter" class="containerCounter">
    <div class="counter">
      <h1>
        {{ counter }}
      </h1>
    </div>
  </div>
</template>

<script>
export default {
  created: function () {
    setInterval(() => this.decreaseCounter(), 1000);
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
.containerCounter {
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  z-index: 1;
  height: 100%;
  width: 100%;
  background-color: black;
}

.counter {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 200px;
  height: 200px;
  background: darkturquoise;
  border-radius: 200px;
  animation: fadeinout 1s linear forwards;
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
