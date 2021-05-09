<template>
  <div id="main-wrapper">
    <div>
      <h1 class="greeting">Hi, let's play Pazaak!</h1>
      <button id="startGame" v-if="!start" @click="play">Lets go!</button>
      <button v-else @click="play">Stop Match</button>
      <p id="matchcount">Matchcount: {{ computerMatch }}:{{ playerMatch }}</p>
      <p class="instructions-link">
        <a href="/about">Learn how to play</a>
      </p>
      <Board
        :isPlaying="start"
        @game-over="play"
        @computer-win="playerMatchPlus"
        @player-win="computerMatchPlus"
      />
    </div>
  </div>
</template>
<script>
import Board from "@/components/Board";
export default {
  data() {
    return {
      start: false,
      playerMatch: 0,
      computerMatch: 0
    };
  },
  components: {
    Board
  },
  methods: {
    play() {
      this.start = !this.start;
    },
    computerMatchPlus() {
      this.computerMatch++;
    },
    playerMatchPlus() {
      this.playerMatch++;
    }
  }
};
</script>
<style scoped lang="scss">
#main-wrapper {
  width: 66%;
  margin: 0 auto;
  //border: 1px solid magenta;
  div {
    position: relative;
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
        margin-right: 1rem;
      }
    }
    button {
      display: block;
      margin: 0 auto;
    }
    #startGame {
      border: 1px solid gold;
    }
    #matchcount {
      position: absolute;
      left: 1rem;
    }
  }
}
@media (max-width: 1650px) {
  #main-wrapper {
    width: 100%;
  }
}
</style>
