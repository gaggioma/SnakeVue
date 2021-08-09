<template>
  <div class="snake" :style="position"></div>
</template>

<script>
export default {
  props: ["positionProps"],

  data: function () {
    return {
      position: this.positionProps,
    };
  },

  watch: {
    positionProps: function (val) {
      this.position = val;
    },
  },
};
</script>

<style>
.snake {
  width: 20px;
  height: 20px;
  background-color: turquoise;
  position: absolute;
}
</style>
