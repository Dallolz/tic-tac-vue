<template>
  <div class="game-body"> 
    <button v-if="!this.startGame" v-on:click="startingGame()"> LET'S BEGIN </button>
    <div class="game-body" v-if="this.startGame">
    <div class="game-status">{{ gameStatus }}</div>
    <div class="game-status">{{ streakRecord }} </div>
    <TicBoard :tics="tics" :OnHandleClick="OnClicked"></TicBoard>
    <button v-if="isResetEnabled" @click="reset()">Reset</button>
    </div>
  </div>
</template>

<script>
import TicBoard from "./Board.vue";

export default {
  name: "Game",
  components: {
    TicBoard
  },
  data: function() {
    return {
      winner: null,
      startGame: false,
      circleWins: 0,
      crossWins: 0,
      isPlayerX: true,
      tics: Array(9).fill("")
    };
  },
  computed: {
    gameStatus: function() {
      if (this.isPlayerX) {
        return "Next Player: X";
      } else {
        return "Next Player: O";
      }
    },
    streakRecord: function() {
      if (this.crossWins > 0 || this.circleWins > 0) {
        return "Circle wins : " + this.circleWins + "   Cross wins: " + this.crossWins;
      } else if (this.winner !== null) {
        return "Winner is " + this.winner;
      }
      else{
        return "No winner yet !"
      }
    },
    isResetEnabled: function() {
      if (this.winner === "X" || this.winner === "O") {
        return true;
      }
      //verif if board full
      for (let i = 0; i < this.tics.length; i++) {
        if (this.tics[i] != "X" && this.tics[i] != "O") {
          return false;
        }
      }

      return true;
    }
  },
  methods: {
    calculateWinner() {
      const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
      ];

      for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (
          this.tics[a] &&
          this.tics[a] === this.tics[b] &&
          this.tics[b] === this.tics[c]
        ) {
          this.winner = this.tics[a];
          if (this.winner === "X") {
            this.crossWins += 1;
          }
          if (this.winner === "O") {
            this.circleWins += 1;
          }
          console.log(this.winner, "c'est lui le winner");
          return;
        }
      }
    },
    OnClicked(event) {
      if (this.winner === "X" || this.winner === "O") {
        return;
      }

      if (this.tics[event] === "X" || this.tics[event] === "O") {
        return;
      }

      this.$set(this.tics, event, this.isPlayerX ? "X" : "O");
      this.isPlayerX = !this.isPlayerX; //swap player
      this.calculateWinner();
    },
    startingGame() {
      this.startGame = true;
    },
    reset() {
      console.log("reset");
      for (let i = 0; i < this.tics.length; i++) {
        this.$set(this.tics, i, "");
      }
      this.winner = null;
    }
  }
};
</script>
