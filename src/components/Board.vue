<template>
  <div id="board-wrapper">
    <h1>{{ isPlaying ? "true" : "false" }}</h1>
    <div id="score">
      <p>Games played: 2</p>
      <p><span>0</span>:<span>2</span></p>
      <p>Opponent: Atton</p>
    </div>
    <div id="table">
      <div class="player-board" id="left-player">
        <div id="count-left" class="count">20</div>
        <div id="left-board" class="board">
          <div v-for="card of boardLength" :key="card" class="card">
            <img :src="isPlaying ? cardsPlus[0] : emptyCard" alt="" />
          </div>
        </div>
        <div id="left-player-hand" class="player-hand">
          <div v-for="card of handSize" :key="card" class="card">
            <img :src="isPlaying ? cardsPlus[0] : emptyCard" alt="" />
          </div>
        </div>
      </div>
      <div class="player-board" id="right-player">
        <div id="count-right" class="count">18</div>
        <div id="right-board" class="board"></div>
        <div id="right-player-hand" class="player-hand"></div>
      </div>
    </div>
  </div>
</template>
<script>
import cp1 from "@/assets/cards/plus1.png";
import cp2 from "@/assets/cards/plus2.png";
import cp3 from "@/assets/cards/plus3.png";
import cp4 from "@/assets/cards/plus4.png";
import cp5 from "@/assets/cards/plus5.png";
import cp6 from "@/assets/cards/plus6.png";

import cm1 from "@/assets/cards/minus1.png";
import cm2 from "@/assets/cards/minus2.png";
import cm3 from "@/assets/cards/minus3.png";
import cm4 from "@/assets/cards/minus4.png";
import cm5 from "@/assets/cards/minus5.png";
import cm6 from "@/assets/cards/minus6.png";

import cpm1 from "@/assets/cards/plmi1.png";
import cpm2 from "@/assets/cards/plmi2.png";
import cpm3 from "@/assets/cards/plmi3.png";
import cpm4 from "@/assets/cards/plmi4.png";
import cpm5 from "@/assets/cards/plmi5.png";
import cpm6 from "@/assets/cards/plmi6.png";

import cn01 from "@/assets/cards/neu01.png";
import cn02 from "@/assets/cards/neu02.png";
import cn03 from "@/assets/cards/neu03.png";
import cn04 from "@/assets/cards/neu04.png";
import cn05 from "@/assets/cards/neu05.png";
import cn06 from "@/assets/cards/neu06.png";
import cn07 from "@/assets/cards/neu07.png";
import cn08 from "@/assets/cards/neu08.png";
import cn09 from "@/assets/cards/neu09.png";
import cn10 from "@/assets/cards/neu10.png";

import hidden from "@/assets/cards/hidden.png";

import empty from "@/assets/cards/empty.png";

const cardsPlus = [cp1, cp2, cp3, cp4, cp5, cp6];
const cardsMinus = [cm1, cm2, cm3, cm4, cm5, cm6];
const cardsPlMi = [cpm1, cpm2, cpm3, cpm4, cpm5, cpm6];
const cardsNeu = [cn01, cn02, cn03, cn04, cn05, cn06, cn07, cn08, cn09, cn10];
const cardHidden = hidden;
const emptyCard = empty;
const boardLength = 15;
const handSize = 5;
export default {
  props: {
    isPlaying: Boolean
  },
  data() {
    return {
      cardsPlus,
      cardsMinus,
      cardsPlMi,
      cardsNeu,
      cardHidden,
      boardLength,
      emptyCard,
      handSize
    };
  },
  methods: {}
};
</script>
<style scoped lang="scss">
#board-wrapper {
  //border: 1px solid green;
  height: 100px;
  display: flex;
  flex-direction: column;
  width: 100%;
  #score {
    justify-self: center;
    align-self: center;
    border: 1px solid gold;
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    width: 100%;
    padding: 1rem 0;
  }
  #table {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr;
    .player-board {
      border: 1px solid teal;
      height: 100px;
      .count {
        border: 1px solid magenta;
      }
      .board {
        border: 1px solid magenta;
        min-height: 500px;
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
        grid-template-rows: 1fr 1fr 1fr;
        align-items: center;
        .card {
          //border: 1px solid gold;
          display: flex;
          justify-content: center;
          img {
            //border: 1px solid green;
          }
        }
      }
      .player-hand {
        border: 1px solid magenta;
        min-height: 200px;
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
        grid-template-rows: 1fr;
        align-items: center;
        .card {
          //border: 1px solid gold;
          display: flex;
          justify-content: center;
        }
      }
    }
  }
}
</style>
