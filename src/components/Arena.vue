<template>
  <div class="container" :style="{ height: arenaHeight + 'px' }">
    <div class="score">Score: {{ score }}</div>

    <div
      tabindex="0"
      class="arena"
      :style="{ height: height + 'px', width: width + 'px' }"
      @keydown="keyPressHandler($event)"
    >
      <Snake
        v-for="snakePosition in snakePositionArray"
        :key="snakePosition['margin-top'] + '_' + snakePosition['margin-left']"
        :positionProps="snakePosition"
      >
      </Snake>

      <Food :positionProps="foodPosition" :foodSizeProps="foodSize"> </Food>
    </div>
  </div>
</template>

<script>
import Snake from "./Snake";
import Food from "./Food";

import { insidePoly } from "./Utils";

export default {
  components: {
    Snake,
    Food,
  },

  created: function () {

    //Enable window resize
    window.addEventListener("resize", this.windowSize);
  
    //Create random food
    this.createRandomFood();

    //Update snake position every updateTime ms
    var updateTime = 80;
    setInterval(() => this.updateSnake(), updateTime);
  },

  data: function () {
    //Init margins
    var topTmp = window.innerHeight * 0.2; // Math.round(500 / 2);
    var leftTmp = window.innerWidth * 0.5; // Math.round(500 / 2);
    var stepSizeTmp = 20;

    //Start with intiLenght snake
    var initLenght = 15;
    var arrayTmp = [];
    for (var i = 0; i < initLenght; i++) {
      arrayTmp.push({
        "margin-top": topTmp + i * stepSizeTmp + "px",
        "margin-left": leftTmp + "px",
      });
    }

    return {
      directionKey: "up",
      snakePositionArray: arrayTmp,
      direction: {
        top: topTmp,
        left: leftTmp,
      },
      stepSize: stepSizeTmp, //Snake dimension
      height: 0.8 * window.innerHeight, //Arena size
      width: 0.8 * window.innerWidth, //Arena size
      arenaHeight: window.innerHeight,
      foodSize: 15, //Food dimensio
      foodPosition: null,
      score: 0,
    };
  },

  watch: {
    direction() {
      //Check if food has been eaten. At least one point must be inside snake.
      var snakePoly = [
        [this.direction.left, this.direction.top],
        [this.direction.left + this.stepSize, this.direction.top],
        [
          this.direction.left + this.stepSize,
          this.direction.top + this.stepSize,
        ],
        [this.direction.left, this.direction.top + this.stepSize],
      ];

      var foodPoints = [
        [this.foodPosition["left"], this.foodPosition["top"]],
        [this.foodPosition["left"] + this.foodSize, this.foodPosition["top"]],
        [this.foodPosition["left"], this.foodPosition["top"] + this.foodSize],
        [
          this.foodPosition["left"] + this.foodSize,
          this.foodPosition["top"] + this.foodSize,
        ],
      ];

      var isInside = false;
      for (var i = 0; i < foodPoints.length; i++) {
        var res = insidePoly(foodPoints[i], snakePoly);
        if (res) {
          isInside = true;
          break;
        }
      }

      if (isInside) {
        //Increase score
        this.score = this.score + 1;

        //New food
        this.createRandomFood();

        //give the last point
        var lastTop = parseInt(
          this.snakePositionArray[this.snakePositionArray.length - 1][
            "margin-top"
          ].replace("px", "")
        );
        var topList = [
          lastTop,
          lastTop + this.stepSize,
          lastTop - this.stepSize,
        ];

        var lastLeft = parseInt(
          this.snakePositionArray[this.snakePositionArray.length - 1][
            "margin-left"
          ].replace("px", "")
        );
        var leftList = [
          lastLeft,
          lastLeft + this.stepSize,
          lastLeft - this.stepSize,
        ];

        //Shift a point and try to just exists
        for (var topIdx = 0; topIdx < topList.length; topList++) {
          for (var leftIdx = 0; leftIdx < leftList.length; leftIdx++) {
            var isPresent = this.snakePositionArray.findIndex((element) => {
              return (
                parseInt(element["margin-top"].replace("px", "")) ===
                  topList[topIdx] &&
                parseInt(element["margin-left"].replace("px", "")) ===
                  leftList[leftIdx]
              );
            });

            if (isPresent == -1) {
              this.snakePositionArray.push({
                "margin-top": topList[topIdx] + "px",
                "margin-left": leftList[leftIdx] + "px",
              });
              break;
            }
          }
        }
      }
    },
  },

  methods: {
    //Remove the last element and create new one on top
    updateSnake() {
      //var arrayTmp = [...this.snakePositionArray]

      //Remove last
      this.snakePositionArray.pop();

      //Create first
      var top = this.direction.top;
      var left = this.direction.left;
      if (this.directionKey === "up") {
        top =
          this.direction.top === 0
            ? this.height - this.stepSize
            : this.direction.top - this.stepSize < 0
            ? 0
            : this.direction.top - this.stepSize;
      } else if (this.directionKey === "down") {
        top =
          this.direction.top === this.height - this.stepSize
            ? 0
            : this.direction.top + 2 * this.stepSize > this.height
            ? this.height - this.stepSize
            : this.direction.top + this.stepSize;
      } else if (this.directionKey === "left") {
        left =
          this.direction.left === 0
            ? this.width - this.stepSize
            : this.direction.left - this.stepSize < 0
            ? 0
            : this.direction.left - this.stepSize;
      } else if (this.directionKey === "right") {
        left =
          this.direction.left === this.width - this.stepSize
            ? 0
            : this.direction.left + 2 * this.stepSize > this.width
            ? this.width - this.stepSize
            : this.direction.left + this.stepSize;
      }

      this.snakePositionArray.unshift({
        "margin-top": top + "px",
        "margin-left": left + "px",
      });

      //Update array of snake
      //this.snakePositionArray = arrayTmp;

      //Update direction
      this.direction = {
        top: top,
        left: left,
      };
    },

    //Key press handler
    keyPressHandler(event) {
      if (event.key == "ArrowUp") {
        this.directionKey =
          this.directionKey !== "down" ? "up" : this.directionKey;
      } else if (event.key == "ArrowDown") {
        this.directionKey =
          this.directionKey !== "up" ? "down" : this.directionKey;
      } else if (event.key == "ArrowLeft") {
        this.directionKey =
          this.directionKey !== "right" ? "left" : this.directionKey;
      } else if (event.key == "ArrowRight") {
        this.directionKey =
          this.directionKey !== "left" ? "right" : this.directionKey;
      }
    },

    //On window resize
    windowSize(val) {
      console.log(val);
      this.arenaHeight = val.currentTarget.innerHeight;
      this.height = val.currentTarget.innerHeight * 0.8;
      this.width = val.currentTarget.innerWidth * 0.8;
    },

    //Bounding check
    /*
        boundingCheck(){
            if(this.marginTop >= this.height){
                this.marginTop = 0;
            }

            if(this.marginTop < 0){
                this.marginTop = this.height;
            }

            if(this.marginLeft >= this.width){
                this.marginLeft = 0;
            }

            if(this.marginLeft < 0){
                this.marginLeft = this.width;
            }
        },
        */

    //Create food position
    createRandomFood() {
      this.foodPosition = {
        top:
          Math.floor(Math.random() * (this.height - 2 * this.foodSize)) +
          this.foodSize,
        left:
          Math.floor(Math.random() * (this.width - 2 * this.foodSize)) +
          this.foodSize,
      };
    },
  },
};
</script>

<style lang="scss">
.arena {
  border-style: solid;
  background-color: black;
}

.score {
  font-size: 20px;
  margin-bottom: 10px;
  color: white;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: cadetblue;
}
</style>
