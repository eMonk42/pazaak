<template>
  <div id="main-wrapper">
    <h1 class="greeting">Hi, let's play Pazaak!</h1>
    <button id="startGame" v-if="!start" @click="play">Lets go!</button>
    <button v-else @click="play">Stop Match</button>
    <p class="instructions-link"><a href="about">Learn how to play</a></p>
    <Board :isPlaying="start" @game-over="play" />
  </div>
</template>
<script>
import Board from "@/components/Board";
export default {
  data() {
    return {
      start: false
    };
  },
  components: {
    Board
  },
  methods: {
    play() {
      this.start = !this.start;
    }
  }
};
</script>
<style scoped lang="scss">
#main-wrapper {
  width: 66%;
  margin: 0 auto;
  //border: 1px solid magenta;
  .greeting {
    text-align: center;
    margin-bottom: 1rem;
    font-size: 1.5rem;
  }
  .instructions-link {
    text-align: end;
    a {
      text-decoration: none;
      color: #bbb;
    }
  }
  button {
    display: block;
    margin: 0 auto;
  }
  #startGame {
    border: 1px solid gold;
  }
}
@media (max-width: 1650px) {
  #main-wrapper {
    width: 100%;
  }
}
</style>
