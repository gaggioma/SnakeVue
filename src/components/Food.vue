<template>
  <div class="food" :style="position"></div>
</template>

<script>
export default {
  props: ["positionProps", "foodSizeProps"],

  data: function () {
    return {
      position: {
        width: this.foodSizeProps + "px",
        height: this.foodSizeProps + "px",
        "border-radius": this.foodSizeProps + "px",
        "margin-top": this.positionProps["top"] + "px",
        "margin-left": this.positionProps["left"] + "px",
      },
    };
  },

  watch: {
    positionProps: function (val) {
      this.position = {
        width: this.foodSizeProps + "px",
        height: this.foodSizeProps + "px",
        "border-radius": this.foodSizeProps + "px",
        "margin-top": val["top"] + "px",
        "margin-left": val["left"] + "px",
      };
    },
  },
};
</script>

<style>
.food {
  background-color: yellowgreen;
  position: absolute;
}
</style>
