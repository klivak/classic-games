<template>
  <div class="game-snake">
    <canvas ref="gameScreen"></canvas>
  </div>
</template>

<script>
export default {
  data() {
    return {
      snake: null,
      snakeLength: null,
      snakeSize: null,
      snakeDirection: null,

      food: null,

      canvas: null,
      context: null,
      screenWidth: null,
      screenHeight: null
    };
  },
  mounted() {
    this.gameInitialize();
    this.snakeInitialize();
    this.foodInitialize();

    setInterval(this.gameLoop, 1000 / 30);
  },
  beforeDestroy() {},
  methods: {
    // scene methods
    gameInitialize() {
      this.canvas = this.$refs.gameScreen;
      this.context = this.canvas.getContext("2d");

      this.screenWidth = window.innerWidth;
      this.screenHeight = window.innerHeight;

      this.canvas.width = this.screenWidth;
      this.canvas.height = this.screenHeight;

      document.addEventListener("keydown", this.keyboardHandler);
    },

    gameLoop() {
      this.gameDraw();
      this.snakeUpdate();
      this.snakeDraw();
      this.foodDraw();
    },

    gameDraw() {
      this.context.fillStyle = "rgb(180, 250, 213)";
      this.context.fillRect(0, 0, this.screenWidth, this.screenHeight);
    },

    // snake methods
    snakeInitialize() {
      this.snake = [];
      this.snakeLength = 15;
      this.snakeSize = 20;
      this.snakeDirection = "down";

      for (let index = this.snakeLength - 1; index >= 0; index--) {
        this.snake.push({
          x: index,
          y: 0
        });
      }
    },
    snakeDraw() {
      for (let index = 0; index < this.snake.length; index++) {
        this.context.fillStyle = "white";
        this.context.fillRect(
          this.snake[index].x * this.snakeSize,
          this.snake[index].y * this.snakeSize,
          this.snakeSize,
          this.snakeSize
        );
      }
    },
    snakeUpdate() {
      let snakeHeadX = this.snake[0].x;
      let snakeHeadY = this.snake[0].y;

      if (this.snakeDirection == "down") {
        snakeHeadY++;
      } else if (this.ssnakeDirection == "right") {
        snakeHeadX++;
      }
      let snakeTail = this.snake.pop();
      snakeTail.x = snakeHeadX;
      snakeTail.y = snakeHeadY;

      this.snake.unshift(snakeTail);
    },

    // food
    foodInitialize() {
      this.food = {
        x: 0,
        y: 0
      };
      this.setFoodPosition();
    },
    foodDraw() {
      this.context.fillStyle = "white";
      this.context.fillRect(
        this.food.x,
        this.food.y,
        this.snakeSize,
        this.snakeSize
      );
    },
    setFoodPosition() {
      let randomX = Math.floor(Math.random() * this.screenWidth);
      let randomY = Math.floor(Math.random() * this.screenHeight);

      this.food.x = randomX;
      this.food.y = randomY;
    },

    // events
    keyboardHandler(event) {
      console.log(event);

      if (event.keyCode == "39") {
          this.snakeDirection = "right";
      } else if (event.keyCode == "40") {
          this.snakeDirection = "down";
      }
    }
  }
};
</script>
  
<style lang="scss" src="./../assets/scss/games/game-snake.scss"></style> 
