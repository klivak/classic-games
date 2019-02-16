<template>
  <div class="game-snake">
    <h2>Game snake</h2>
    <canvas ref="gameScreen"></canvas>
  </div>
</template>

<script>
export default {
  data() {
    return {
      snake: null,
      context: null,
      screenWidth: null,
      screenHeight: null
    };
  },
  mounted() {
    this.getInitialize();
    console.log(this.snake);
  },
  methods: {
    getInitialize() {
      this.snake = this.$refs.gameScreen;
      this.context = this.snake.getContext("2d");
    },
    gameLoop() {},
    gameDraw() {}
  }
};
</script>
  
<style lang="scss" src="./../assets/scss/games/game-snake.scss"></style> 
