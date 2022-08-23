<template>
  <div id="app">
    <div class="wrapper clearfix">
      <Players
        v-bind:activePlayer="activePlayer"
        v-bind:scoresPlayer="scoresPlayer"
        v-bind:currentScore="currentScore"
        v-bind:isWinner="isWinner"
      />
      <Controls
        v-on:handleNewGame="handleNewGame"
        v-on:handleRollDice="handleRollDice"
        v-on:handleHoldScore="handleHoldScore"
        v-bind:finalScore="finalScore"
        v-on:handleChangeFinalScore="handleChangeFinalScore"
        v-bind:isPlaying="isPlaying"
      />
      <Dices v-bind:dices="dices" />
      <PopupRule
        v-bind:isOpenPopup="isOpenPopup"
        v-on:handleConfirm="handleConfirm"
      />
    </div>
  </div>
</template>

<script>
import Players from "./components/Players.vue";
import Controls from "./components/Controls.vue";
import Dices from "./components/Dices.vue";
import PopupRule from "./components/PopupRule.vue";
export default {
  name: "app",
  data() {
    return {
      isPlaying: false,
      activePlayer: 1,
      scoresPlayer: [0, 0],
      currentScore: 10,
      isOpenPopup: false,
      dices: [1, 1],
      finalScore: 16
    };
  },
  components: { Players, Controls, Dices, PopupRule, PopupRule },
  computed: {
    isWinner() {
      let { scoresPlayer, finalScore } = this;

      if (scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore) {
        //dung cuoc choi
        this.isPlaying = false;
        return true;
      }

      return false;
    }
  },
  methods: {
    handleNewGame() {
      console.log("new game App.vue");
      this.isOpenPopup = true;
    },
    nextPlayer() {
      this.activePlayer = this.activePlayer === 0 ? 1 : 0;
      this.currentScore = 0;
    },
    handleConfirm() {
      console.log("handleConfirm in App");
      this.isPlaying = true;
      this.isOpenPopup = false;
      this.activePlayer = 0;
      this.scoresPlayer = [0, 0];
      this.currentScore = 0;
      this.dices = [1, 1];
    },
    handleRollDice() {
      console.log("handleRollDice in App");
      if (this.isPlaying) {
        //xoay xuc xac
        var dice1 = Math.floor(Math.random() * 6) + 1;
        var dice2 = Math.floor(Math.random() * 6) + 1;

        this.dices = [dice1, dice2];

        if (dice1 === 1 || dice2 === 1) {
          //doi luot choi, reset currentScore = 0
          setTimeout(() => {
            //this nay chinh la vue
            alert(`Player ${this.activePlayer} da quay trung so1, rat tiec`);
          }, 10);
          this.nextPlayer();
        } else {
          //cong don diem tam thoi cho nguoi choi
          this.currentScore = this.currentScore + dice1 + dice2;
        }
      } else {
        alert("Please enter new game first");
      }
    },
    handleHoldScore() {
      if (this.isPlaying) {
        let { scoresPlayer, activePlayer, currentScore } = this;
        let oldScore = scoresPlayer[activePlayer];

        let cloneScoresPlayer = [...scoresPlayer];
        cloneScoresPlayer[activePlayer] = oldScore + currentScore;

        this.scoresPlayer = cloneScoresPlayer;

        if (!this.isWinner) {
          this.nextPlayer();
        }
      } else {
        alert("Please enter new game first");
      }
    },
    handleChangeFinalScore(event) {
      var number = parseInt(event.target.value);

      if (isNaN(number)) {
        this.finalScore = "";
      } else {
        this.finalScore = number;
      }
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.clearfix::after {
  content: "";
  display: table;
  clear: both;
}

body {
  background-image: linear-gradient(
      rgba(62, 20, 20, 0.4),
      rgba(62, 20, 20, 0.4)
    ),
    url("./assets/back.jpg");
  background-size: cover;
  background-position: center;
  font-family: Lato;
  font-weight: 300;
  position: relative;
  height: 100vh;
  color: #555;
}

.wrapper {
  width: 1000px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}
</style>
