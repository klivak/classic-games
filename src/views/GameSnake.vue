<template>
  <div class="game-snake">
    <section ref="gameOverMenu" id="game-over">
      <h2 class="title">GAME OVER!</h2>
      <h3 id="restart-button">Restart ?</h3>
    </section>
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
        screenHeight: null,

        gameState: null,
        gameOverMenu: null
      };
    },
    mounted() {
      this.gameInitialize();
      this.snakeInitialize();
      this.foodInitialize();

      setInterval(this.gameLoop, 1000 / 30);
    },
    beforeDestroy() {
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

        document.addEventListener("keydown", this.keyboardHandler);

        this.gameOverMenu = this.$refs.gameOverMenu;

        this.setState('PLAY');
      },

      gameLoop() {
        this.gameDraw();
        if (this.gameState === 'PLAY') {
          this.snakeUpdate();
          this.snakeDraw();
          this.foodDraw();
        }

      },

      gameDraw() {
        this.context.fillStyle = "rgb(180, 250, 213)";
        this.context.fillRect(0, 0, this.screenWidth, this.screenHeight);
      },

      // snake methods
      snakeInitialize() {
        this.snake = [];
        this.snakeLength = 3;
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
        } else if (this.snakeDirection == "right") {
          snakeHeadX++;
        } else if (this.snakeDirection == "top") {
          snakeHeadY--;
        } else if (this.snakeDirection == "left") {
          snakeHeadX--;
        }

        this.checkFoodCollisions(snakeHeadX, snakeHeadY);
        this.checkWallCollisions(snakeHeadX, snakeHeadY);


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
          this.food.x * this.snakeSize,
          this.food.y * this.snakeSize,
          this.snakeSize,
          this.snakeSize
        );
      },
      setFoodPosition() {
        let randomX = Math.floor(Math.random() * this.screenWidth);
        let randomY = Math.floor(Math.random() * this.screenHeight);

        this.food.x = Math.floor(randomX / this.snakeSize);
        this.food.y = Math.floor(randomY / this.snakeSize);
      },

      // events
      keyboardHandler(event) {
        console.log(event);

        if (event.keyCode == "39" && this.snakeDirection != "left") {
          this.snakeDirection = "right";
        } else if (event.keyCode == "40" && this.snakeDirection != "up") {
          this.snakeDirection = "down";
        } else if (event.keyCode == "37" && this.snakeDirection != "right") {
          this.snakeDirection = "left";
        } else if (event.keyCode == "38" && this.snakeDirection != "down") {
          this.snakeDirection = "top";
        }
      },

      // collision handling
      checkFoodCollisions(snakeHeadX, snakeHeadY) {
        if (snakeHeadX == this.food.x && snakeHeadY == this.food.y) {
          console.log("FOOOD");
          this.snake.push({
            x: 0,
            y: 0
          });
          this.snakeLength++;
        }
      },
      checkWallCollisions(snakeHeadX, snakeHeadY) {
        if (snakeHeadX * this.snakeSize >= this.screenWidth || snakeHeadX * this.snakeSize < 0) {
          console.log('Wall found');
          this.setState('GAME OVER');
        }
      },


      // game state handling
      setState(state) {
        this.gameState = state;
        this.showMenu(state);
      },
      displayMenu(menu) {
        menu.style.visibility = 'visible';
      },
      showMenu(state) {
        if (state === 'GAME OVER') {
          this.displayMenu(this.gameOverMenu);
        }
      }
    }
  };
</script>

<style lang="scss" src="./../assets/scss/games/game-snake.scss"></style> 


