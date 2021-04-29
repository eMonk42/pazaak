<template>
  <div id="board-wrapper">
    <!-- <h1>{{ isPlaying ? "true" : "false" }}</h1> -->
    <div id="score">
      <p>Games played: {{ gameCount }}</p>
      <p>
        <span>{{ matchWinsPlayer }}</span
        >:<span>{{ matchWinsComputer }}</span>
      </p>
      <p>Opponent: Atton</p>
    </div>
    <div id="table">
      <div class="player-board" id="left-player">
        <div id="count-left" class="count">{{ countPointsPlayer }}</div>
        <div id="left-board" class="board">
          <div v-if="!handReady" class="boardcards">
            <div v-for="card of boardLength" :key="card" class="card">
              <img :src="emptyCard" alt="" />
            </div>
          </div>
          <div v-else class="boardcards">
            <div v-for="(card, index) of boardLength" :key="index" class="card">
              <img
                :src="
                  playerCardsOnBoard[index]
                    ? playerCardsOnBoard[index]
                    : emptyCard
                "
                alt=""
              />
            </div>
          </div>
        </div>

        <p>Your Hand</p>
        <div id="left-player-hand" class="player-hand">
          <div v-if="!handReady" class="handcards">
            <div v-for="card of handSize" :key="card" class="card">
              <img :src="emptyCard" alt="" />
            </div>
          </div>
          <div v-else class="handcards">
            <div v-for="(card, index) of handSize" :key="index" class="card">
              <img
                :src="playerHand[index] ? playerHand[index] : emptyCard"
                alt=""
                @click="playCard(index)"
              />
            </div>
          </div>
        </div>
      </div>
      <div class="player-board" id="right-player">
        <div id="count-right" class="count">{{ countPointsComputer }}</div>
        <div id="right-board" class="board">
          <div v-if="!handReady" class="boardcards">
            <div v-for="card of boardLength" :key="card" class="card">
              <img :src="emptyCard" alt="" />
            </div>
          </div>
          <div v-else class="boardcards">
            <div v-for="(card, index) of boardLength" :key="index" class="card">
              <img
                :src="
                  computerCardsOnBoard[index]
                    ? computerCardsOnBoard[index]
                    : emptyCard
                "
                alt=""
              />
            </div>
          </div>
        </div>
        <p>Opponent's Hand</p>
        <div id="right-player-hand" class="player-hand">
          <div v-if="!handReady" class="handcards">
            <div v-for="card of handSize" :key="card" class="card">
              <img :src="emptyCard" alt="" />
            </div>
          </div>
          <div v-else class="handcards">
            <div v-for="(card, index) of handSize" :key="index" class="card">
              <img :src="computerHand[index] ? cardHidden : emptyCard" alt="" />
            </div>
          </div>
        </div>
      </div>
    </div>
    <div id="controls">
      <div class="buttons">
        <button
          @click="nextRound"
          :class="buttonDisabled && isPlaying ? 'clickable' : ''"
        >
          Pass
        </button>
        <button
          @click="playerHolds"
          :class="buttonDisabled && isPlaying ? 'clickable' : ''"
        >
          Hold
        </button>
      </div>
      <div class="log">
        <p id="computer-log">Atton: Hello, my name is Atton</p>
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

import Swal from "sweetalert2";

const cardsPlus = [cp1, cp2, cp3, cp4, cp5, cp6];
const cardsMinus = [cm1, cm2, cm3, cm4, cm5, cm6];
const cardsPlMi = [cpm1, cpm2, cpm3, cpm4, cpm5, cpm6];
const cardsNeu = [cn01, cn02, cn03, cn04, cn05, cn06, cn07, cn08, cn09, cn10];
const cardHidden = hidden;
const emptyCard = empty;
const boardLength = 15;
const handSize = 5;
let playerHand = [];
let computerHand = [];
let playerCardsOnBoard = [];
let computerCardsOnBoard = [];
let handReady = false;
let countPointsPlayer = 0;
let countPointsComputer = 0;
let playerIsHolding = false;
let computerIsHolding = false;
let gameCount = 0;
let matchWinsPlayer = 0;
let matchWinsComputer = 0;
let buttonDisabled = true;
const swal = Swal;
let cardPlayed = false;
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
      handSize,
      playerHand,
      computerHand,
      playerCardsOnBoard,
      computerCardsOnBoard,
      handReady,
      countPointsPlayer,
      countPointsComputer,
      playerIsHolding,
      computerIsHolding,
      gameCount,
      matchWinsPlayer,
      matchWinsComputer,
      buttonDisabled,
      swal,
      cardPlayed
    };
  },
  methods: {
    getHandCardsPlayer(array) {
      array = [];
      let allPossibleCards = this.cardsPlus
        .concat(this.cardsMinus)
        .concat(this.cardsPlMi);
      for (let i = 0; i < this.handSize; i++) {
        array.push(
          allPossibleCards[
            Math.round(Math.random() * (allPossibleCards.length - 1))
          ]
        );
      }
      this.playerHand = array;
    },
    getHandCardsComputer(array) {
      array = [];
      let allPossibleCards = this.cardsPlus
        .concat(this.cardsMinus)
        .concat(this.cardsPlMi);
      for (let i = 0; i < this.handSize; i++) {
        array.push(
          allPossibleCards[
            Math.round(Math.random() * (allPossibleCards.length - 1))
          ]
        );
      }
      this.computerHand = array;
    },
    win() {
      this.computerIsHolding = false;
      this.playerIsHolding = false;
      this.buttonDisabled = true;
      this.matchWinsPlayer++;
      this.gameCount++;
      this.countPointsPlayer = 0;
      this.countPointsComputer = 0;
      this.playerCardsOnBoard = [];
      this.computerCardsOnBoard = [];
      if (this.matchWinsPlayer == 2 || this.matchWinsComputer == 2) {
        document.getElementById("computer-log").innerText = "Atton: Good Game";
        this.$emit("game-over");
        swal
          .fire({
            title: "You won the Match!",
            text: "Incredible! You rock! Do you want to play again?",
            width: "400px",
            background: "#1b1b1b",
            showConfirmButton: true,
            showCancelButton: true,
            cancelButtonText: "Maybe later",
            confirmButtonText: "Absolutely!"
          })
          .then(res => {
            if (res.isConfirmed) {
              this.$emit("game-over");
            }
          });
        this.gameCount = 0;
        this.matchWinsComputer = 0;
        this.matchWinsPlayer = 0;
      } else {
        swal.fire({
          title: "You won this game!",
          text: "We are playing Bo3 tho, so keep your concentration up!",
          width: "400px",
          background: "#1b1b1b",
          showConfirmButton: true,
          confirmButtonText: "Nice!"
        });
      }
    },
    lose() {
      this.computerIsHolding = false;
      this.playerIsHolding = false;
      this.buttonDisabled = true;
      this.matchWinsComputer++;
      this.gameCount++;
      this.countPointsPlayer = 0;
      this.countPointsComputer = 0;
      this.playerCardsOnBoard = [];
      this.computerCardsOnBoard = [];
      if (this.matchWinsPlayer == 2 || this.matchWinsComputer == 2) {
        this.$emit("game-over");
        document.getElementById("computer-log").innerText = "Atton: Good Game";
        swal
          .fire({
            title: "You lost the Match!",
            text:
              "Don't be sad! It's a very high variance-based one. Just keep trying! Do you want to try again?",
            width: "400px",
            background: "#1b1b1b",
            showConfirmButton: true,
            showCancelButton: true,
            cancelButtonText: "Maybe later",
            confirmButtonText: "Absolutely!"
          })
          .then(res => {
            if (res.isConfirmed) {
              //this.isPlaying = true;
              this.$emit("game-over");
            }
          });
        this.gameCount = 0;
        this.matchWinsComputer = 0;
        this.matchWinsPlayer = 0;
      } else {
        swal.fire({
          title: "You lost this game!",
          text: "We are playing Bo3 so don't give up just now!",
          width: "400px",
          background: "#1b1b1b",
          showConfirmButton: true,
          confirmButtonText: "Very well"
        });
      }
    },
    draw() {
      this.computerIsHolding = false;
      this.playerIsHolding = false;
      this.buttonDisabled = true;
      this.playerCardsOnBoard = [];
      this.computerCardsOnBoard = [];
      this.gameCount++;
      this.countPointsPlayer = 0;
      this.countPointsComputer = 0;
      document.getElementById("computer-log").innerText = "Atton: Good Game";
      swal.fire({
        title: "Draw! What a game!",
        text: "Good luck in the next one!",
        width: "400px",
        background: "#1b1b1b",
        showConfirmButton: true,
        confirmButtonText: "Next game!"
      });
    },
    checkWinCon() {
      if (this.computerIsHolding && this.playerIsHolding) {
        if (this.countPointsPlayer === this.countPointsComputer) {
          this.draw();
        } else if (
          this.countPointsComputer > 20 &&
          this.countPointsPlayer <= 20
        ) {
          this.win();
        } else if (
          this.countPointsPlayer > 20 &&
          this.countPointsComputer <= 20
        ) {
          this.lose();
        } else if (
          this.countPointsPlayer > 20 &&
          this.countPointsComputer > 20
        ) {
          this.draw();
        } else if (this.countPointsComputer > this.countPointsPlayer) {
          this.lose();
        } else if (this.countPointsComputer < this.countPointsPlayer) {
          this.win();
        }
      } else if (this.countPointsComputer > 20) {
        this.win();
      } else if (this.countPointsPlayer > 20) {
        this.lose();
      }
    },
    computerTurn() {
      //console.log("computerturn here");
      if (!this.buttonDisabled) {
        //this.checkWinCon();
        this.computerCardsOnBoard.push(
          this.cardsNeu[Math.round(Math.random() * (this.cardsNeu.length - 1))]
        );
        this.countPointsComputer +=
          this.cardsNeu.indexOf(
            this.computerCardsOnBoard[this.computerCardsOnBoard.length - 1]
          ) + 1;
        //--------------------- equal to 20 --------------------------------------
        if (this.countPointsComputer === 20) {
          this.computerIsHolding = true;
          document.getElementById("computer-log").innerText =
            "Atton is holding";
          //---------------------- PLayer is Holding and AI is already better ---------------------
        } else if (
          this.playerIsHolding &&
          this.countPointsComputer > this.countPointsPlayer &&
          this.countPointsComputer <= 20
        ) {
          this.computerIsHolding = true;
          document.getElementById("computer-log").innerText =
            "Atton says Good Game! and holds.";
          //------------------------ More than 20 ----------------------------
        } else if (this.countPointsComputer > 20) {
          console.log("MinusCheck here");
          this.computerHand.forEach((card, i) => {
            if (
              this.cardsMinus.indexOf(card) != -1 &&
              -(this.cardsMinus.indexOf(card) + 1) + this.countPointsComputer <=
                20 &&
              !this.cardPlayed
            ) {
              this.computerCardsOnBoard.push(card);
              this.cardPlayed = true;
              this.countPointsComputer -= this.cardsMinus.indexOf(card) + 1;
              this.computerHand.splice(i, 1);
              this.computerIsHolding = true;
              document.getElementById("computer-log").innerText =
                "Atton plays a -" +
                (this.cardsMinus.indexOf(card) + 1) +
                " and holds.";
            } else if (
              this.cardsPlMi.indexOf(card) != -1 &&
              -(this.cardsPlMi.indexOf(card) + 1) + this.countPointsComputer <=
                20 &&
              !this.cardPlayed
            ) {
              this.computerCardsOnBoard.push(card);
              this.cardPlayed = true;
              this.countPointsComputer -= this.cardsPlMi.indexOf(card) + 1;
              this.computerHand.splice(i, 1);
              this.computerIsHolding = true;
              document.getElementById("computer-log").innerText =
                "Atton plays a +/- " +
                (this.cardsPlMi.indexOf(card) + 1) +
                " as a -" +
                (this.cardsPlMi.indexOf(card) + 1) +
                " and holds.";
            }
          });
          //check if a small positive card can do it
          if (this.countPointsComputer != 20 && this.computerHand.length > 0) {
            this.computerHand.forEach((card, i) => {
              if (
                this.cardsPlus.indexOf(card) != -1 &&
                this.cardsPlus.indexOf(card) + 1 + this.countPointsComputer <=
                  20 &&
                this.cardsPlus.indexOf(card) + 1 + this.countPointsComputer >
                  this.countPointsPlayer
              ) {
                this.computerCardsOnBoard.push(card);
                this.countPointsComputer += this.cardsPlus.indexOf(card) + 1;
                //console.log(this.computerHand);
                //console.log(card, this.countPointsComputer);
                document.getElementById("computer-log").innerText =
                  "Atton plays a +" +
                  (this.cardsPlus.indexOf(card) + 1) +
                  " and holds.";
                this.computerHand.splice(i, 1);
                this.computerIsHolding = true;
                this.cardPlayed = true;
              } else if (
                this.cardsPlMi.indexOf(card) != -1 &&
                this.cardsPlMi.indexOf(card) + 1 + this.countPointsComputer <=
                  20 &&
                this.cardsPlus.indexOf(card) + 1 + this.countPointsComputer >
                  this.countPointsPlayer
              ) {
                this.computerCardsOnBoard.push(card);
                this.countPointsComputer += this.cardsPlMi.indexOf(card) + 1;
                //console.log(this.computerHand);
                //console.log(card, this.countPointsComputer);
                document.getElementById("computer-log").innerText =
                  "Atton plays a +/- " +
                  (this.cardsPlMi.indexOf(card) + 1) +
                  " as +" +
                  (this.cardsPlMi.indexOf(card) + 1) +
                  " and holds.";
                this.computerHand.splice(i, 1);
                this.computerIsHolding = true;
                this.cardPlayed = true;
              }
            });
          }
          //------------------------- Less than 20 and bigger than 10 --------------------------
        } else if (
          this.countPointsComputer < 20 &&
          this.countPointsComputer >= 10
        ) {
          this.computerHand.forEach((card, i) => {
            if (
              this.cardsPlus.indexOf(card) + 1 + this.countPointsComputer ==
                20 &&
              !this.cardPLayed &&
              this.cardsPlus.indexOf(card) !== -1
            ) {
              this.computerCardsOnBoard.push(card);
              this.countPointsComputer += this.cardsPlus.indexOf(card) + 1;
              //console.log(this.computerHand);
              //console.log(card, this.countPointsComputer);
              document.getElementById("computer-log").innerText =
                "Atton plays a +" +
                (this.cardsPlus.indexOf(card) + 1) +
                " and holds.";
              this.computerHand.splice(i, 1);
              this.computerIsHolding = true;
              this.cardPlayed = true;
            } else if (
              this.cardsPlMi.indexOf(card) + 1 + this.countPointsComputer ==
                20 &&
              !this.cardPlayed &&
              this.cardsPlMi.indexOf(card) !== -1
            ) {
              this.computerCardsOnBoard.push(card);
              this.countPointsComputer += this.cardsPlMi.indexOf(card) + 1;
              //console.log(this.computerHand);
              //console.log(card, this.countPointsComputer);
              document.getElementById("computer-log").innerText =
                "Atton plays a +/- " +
                (this.cardsPlMi.indexOf(card) + 1) +
                " as +" +
                (this.cardsPlMi.indexOf(card) + 1) +
                " and holds.";
              this.computerHand.splice(i, 1);
              this.computerIsHolding = true;
              this.cardPlayed = true;
            }
          });
          if (!this.cardPlayed) {
            if (this.countPointsComputer >= 18 && !this.playerIsHolding) {
              this.computerIsHolding = true;
              document.getElementById("computer-log").innerText =
                "Atton is holding";
            } else {
              document.getElementById("computer-log").innerText =
                "Atton is passing";
            }
          }
        } else {
          document.getElementById("computer-log").innerText =
            "Atton is passing";
        }
        //-------------------------------------------------
        this.cardPlayed = false;
        //-------------------------------------------------
        setTimeout(() => {
          this.checkWinCon();
        }, 1000);
      }
    },
    playerTurn() {
      //console.log("got here");
      setTimeout(() => {
        //console.log("playerturn here");
        this.playerCardsOnBoard.push(
          this.cardsNeu[Math.round(Math.random() * (this.cardsNeu.length - 1))]
        );
        this.countPointsPlayer +=
          this.cardsNeu.indexOf(
            this.playerCardsOnBoard[this.playerCardsOnBoard.length - 1]
          ) + 1;
        this.buttonDisabled = true;
      }, 1000);
    },
    nextRound() {
      if (this.isPlaying) {
        if (this.buttonDisabled) {
          let gameCountToPreventPLayerTurnOnGameEnd = this.gameCount;
          this.buttonDisabled = false;
          this.checkWinCon();
          //console.log("nextRound here");
          if (this.computerIsHolding === false) {
            this.computerTurn();
          }
          if (
            this.playerIsHolding === false &&
            this.gameCount === gameCountToPreventPLayerTurnOnGameEnd
          ) {
            this.playerTurn();
            //this.buttonDisabled = true;
          }
        }
      }
    },
    playerHolds() {
      this.playerIsHolding = true;
      this.buttonDisabled = false;
      this.checkWinCon();
      if (this.playerIsHolding) {
        setTimeout(() => {
          if (this.playerIsHolding) {
            this.computerTurn();
          }
          if (this.playerIsHolding) {
            setTimeout(() => {
              if (this.playerIsHolding) {
                this.checkWinCon();
                this.computerTurn();
              }
              if (this.playerIsHolding) {
                setTimeout(() => {
                  if (this.playerIsHolding) {
                    this.checkWinCon();
                    this.computerTurn();
                  }
                  if (this.playerIsHolding) {
                    setTimeout(() => {
                      this.checkWinCon();
                      this.computerTurn();
                      if (this.playerIsHolding) {
                        setTimeout(() => {
                          this.checkWinCon();
                          this.computerTurn();
                          this.computerIsHolding = true;
                          this.checkWinCon();
                        }, 1000);
                      }
                    }, 1000);
                  }
                }, 1000);
              }
            }, 1000);
          }
        }, 1000);
      }
    },
    playCard(index) {
      if (this.buttonDisabled) {
        if (this.cardsPlus.indexOf(this.playerHand[index]) !== -1) {
          this.playerCardsOnBoard.push(this.playerHand[index]);
          this.countPointsPlayer +=
            this.cardsPlus.indexOf(this.playerHand[index]) + 1;
          this.playerHand.splice(index, 1);
        } else if (this.cardsMinus.indexOf(this.playerHand[index]) !== -1) {
          this.playerCardsOnBoard.push(this.playerHand[index]);
          this.countPointsPlayer -=
            this.cardsMinus.indexOf(this.playerHand[index]) + 1;
          this.playerHand.splice(index, 1);
        } else if (this.cardsPlMi.indexOf(this.playerHand[index]) !== -1) {
          swal
            .fire({
              title: "<strong>This is a dualcard</strong>",
              html: "<p>Wich side of the card do you want to play?</p>",
              showCancelButton: true,
              focusConfirm: false,
              confirmButtonText: 'Plus <i class="fas fa-plus"></i>',
              cancelButtonText: 'Minus <i class="fas fa-minus"></i>',
              confirmButtonAriaLabel: "Yes",
              cancelButtonAriaLabel: "No",
              width: "400px",
              background: "#1b1b1b"
            })
            .then(result => {
              if (result.isConfirmed) {
                //console.log("true", result.isConfirmed);
                this.countPointsPlayer +=
                  this.cardsPlMi.indexOf(this.playerHand[index]) + 1;
                this.playerCardsOnBoard.push(this.playerHand[index]);
                this.playerHand.splice(index, 1);
              } else {
                //console.log("false", result.isDenied);
                this.countPointsPlayer -=
                  this.cardsPlMi.indexOf(this.playerHand[index]) + 1;
                this.playerCardsOnBoard.push(this.playerHand[index]);
                this.playerHand.splice(index, 1);
              }
            });
        }
      }
    }
  },
  watch: {
    isPlaying: function() {
      if (!this.isPlaying) {
        this.getHandCardsPlayer(this.playerHand);
        this.getHandCardsComputer(this.computerHand);
        this.handReady = !this.handReady;
        document.getElementById("computer-log").innerText =
          "Thanks for playing!";
      } else {
        this.handReady = !this.handReady;
        this.countPointsPlayer = 0;
        this.countPointsComputer = 0;
        this.playerCardsOnBoard = [];
        this.computerCardsOnBoard = [];
        this.playerIsHolding = false;
        this.computerIsHolding = false;
        this.buttonDisabled = true;
        document.getElementById("computer-log").innerText = "Atton: Let's go!";
      }
    }
  },
  mounted() {
    this.getHandCardsPlayer(this.playerHand);
    this.getHandCardsComputer(this.computerHand);
    if (window.innerWidth < 1101) {
      this.boardLength = 9;
    }
  }
};
</script>
<style scoped lang="scss">
#board-wrapper {
  border-bottom: 1px solid #444;
  //border: 1px solid green;
  display: flex;
  flex-direction: column;
  width: 100%;
  #score {
    border: 1px solid #444;
    justify-self: center;
    align-self: center;
    //border: 1px solid gold;
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    width: 100%;
    padding: 0.5rem 0;
    background-color: #1a1a1a;
  }
  #controls {
    display: flex;
    justify-content: space-evenly;
    //border: 1px solid grey;
    background-color: #151515;
    .buttons {
      border: 1px solid #444;
      display: flex;
      justify-content: space-evenly;
      width: 50%;
      padding: 0.5rem 0;
    }
    .log {
      border: 1px solid #444;
      width: 50%;
      padding: 0.5rem;
      #computer-log {
        overflow: auto;
        min-height: 2rem;
      }
    }
  }
  #table {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr;
    .player-board {
      border-right: 1px solid #444;
      border-left: 1px solid #444;
      //border: 1px solid teal;
      #count-left {
        text-align: end;
      }
      .count {
        padding: 0 1rem;
        background-color: #151515;
        border-bottom: 1px solid #444;
      }
      .board {
        //border: 1px solid magenta;
        min-height: 500px;
        .boardcards {
          display: grid;
          grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
          grid-template-rows: 1fr 1fr 1fr;
          align-items: center;
          min-height: 500px;
          .card {
            display: flex;
            justify-content: center;
            //border: 1px solid gold;
          }
        }
      }
      p {
        text-align: center;
        border-top: 1px solid #444;
        background-color: #151515;
      }
      .player-hand {
        //border: 1px solid magenta;
        display: flex;
        align-items: center;
        min-height: 180px;
        background-color: #1a1a1a;
        border-top: 1px solid #444;
        .handcards {
          display: grid;
          grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
          grid-template-rows: 1fr;
          align-items: center;
          width: 100%;
          .card {
            //border: 1px solid gold;
            display: flex;
            justify-content: center;
          }
        }
      }
    }
  }
}
@media (max-width: 1650px) {
  #board-wrapper {
    #table {
      .player-board {
        .board {
          .boardcards {
            grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
            grid-template-rows: 1fr 1fr 1fr;
          }
        }
        .player-hand {
          .handcards {
            grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
            grid-template-rows: 1fr;
          }
        }
      }
    }
  }
}
@media (max-width: 1100px) {
  #board-wrapper {
    #table {
      .player-board {
        .board {
          .boardcards {
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 1fr 1fr 1fr;
          }
        }
        .player-hand {
          .handcards {
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 1fr;
            .card {
              padding: 1rem 0;
            }
          }
        }
      }
    }
  }
}
@media (max-width: 640px) {
  #board-wrapper {
    #table {
      .player-board {
        .board {
          min-height: 100px;
          .boardcards {
            min-height: 100px;
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 120px 120px 120px;
            .card {
              img {
                max-width: 80px;
              }
            }
          }
        }
        .player-hand {
          .handcards {
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 120px 120px;
            //justify-content: space-around;
            .card {
              //padding: 1rem 0;
              img {
                max-width: 80px;
              }
            }
          }
        }
      }
    }
  }
}
@media (max-width: 520px) {
  #board-wrapper {
    #table {
      .player-board {
        .board {
          min-height: 100px;
          .boardcards {
            min-height: 100px;
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 120px 120px 120px;
            .card {
              img {
                max-width: 70px;
              }
            }
          }
        }
        .player-hand {
          .handcards {
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 120px 120px;
            //justify-content: space-around;
            .card {
              //padding: 1rem 0;
              img {
                max-width: 70px;
              }
            }
          }
        }
      }
    }
  }
}
@media (max-width: 480px) {
  #board-wrapper {
    #table {
      .player-board {
        .board {
          min-height: 100px;
          .boardcards {
            min-height: 100px;
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 100px 100px 100px;
            .card {
              img {
                max-width: 60px;
              }
            }
          }
        }
        .player-hand {
          .handcards {
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 100px 100px;
            //justify-content: space-around;
            .card {
              //padding: 1rem 0;
              img {
                max-width: 60px;
              }
            }
          }
        }
      }
    }
  }
}
@media (max-width: 385px) {
  #board-wrapper {
    #table {
      .player-board {
        .board {
          min-height: 100px;
          .boardcards {
            min-height: 100px;
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 80px 80px 80px;
            .card {
              img {
                max-width: 50px;
              }
            }
          }
        }
        .player-hand {
          .handcards {
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 80px 80px;
            //justify-content: space-around;
            .card {
              //padding: 1rem 0;
              img {
                max-width: 50px;
              }
            }
          }
        }
      }
    }
  }
}
@media (max-width: 320px) {
  #board-wrapper {
    #table {
      .player-board {
        .board {
          min-height: 100px;
          .boardcards {
            min-height: 100px;
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 70px 70px 70px;
            .card {
              img {
                max-width: 40px;
              }
            }
          }
        }
        .player-hand {
          .handcards {
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 70px 70px;
            //justify-content: space-around;
            .card {
              //padding: 1rem 0;
              img {
                max-width: 40px;
              }
            }
          }
        }
      }
    }
  }
}
</style>
