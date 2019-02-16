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
  methods: {
    // scene methods
    gameInitialize() {
      this.canvas = this.$refs.gameScreen;
      this.context = this.canvas.getContext("2d");

      this.screenWidth = window.innerWidth;
      this.screenHeight = window.innerHeight;

      this.canvas.width = this.screenWidth;
      this.canvas.height = this.screenHeight;
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

      snakeHeadX++;

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
    },
    foodDraw() {
        this.context.fillStyle = "white";
        this.context.fillRect(this.food.x, this.food.y, this.snakeSize, this.snakeSize);
    }
  }
};
</script>
  
<style lang="scss" src="./../assets/scss/games/game-snake.scss"></style> 
