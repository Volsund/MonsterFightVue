<template>
  <div class="container">
    <section class="players d-flex my-4">
      <div class="player">
        <h4 class="player-name text-center">YOU</h4>
        <div class="health-bar">
          <p class="health">{{ playerHealth }}</p>
          <b-progress
            class="bootstrap-health"
            :value="playerHealth"
            :max="200"
            variant="success"
            animated
          ></b-progress>

          <div class="health-empty"></div>
        </div>
      </div>
      <div class="player">
        <h4 class="player-name text-center">MONSTER</h4>
        <div class="health-bar">
          <p class="health">{{ monsterHealth }}</p>
          <b-progress
            class="bootstrap-health"
            :value="monsterHealth"
            :max="200"
            variant="success"
            animated
          ></b-progress>
          <div class="health-empty"></div>
        </div>
      </div>
    </section>
    <b-card class="mb-2 text-center d-flex">
      <b-button
        :disabled="gameEnded"
        href="#"
        variant="danger"
        class="mr-2 attack-button"
        v-on:click="attack"
        >Attack</b-button
      >
      <b-button
        :disabled="gameEnded"
        href="#"
        variant="warning"
        class="mr-2 special-attack-button"
        v-on:click="specialAttack"
        >Special Attack</b-button
      >
      <b-button
        :disabled="gameEnded"
        href="#"
        variant="success"
        class="mr-2 heal-button"
        v-on:click="heal"
        >Heal</b-button
      >
      <b-button
        :disabled="gameEnded"
        href="#"
        variant="outline-secondary"
        class="mr-2 give-up-button"
        v-on:click="giveUp"
        >Give Up</b-button
      >

      <b-button
        href="#"
        variant="outline-secondary"
        class="start-new-game-button"
        v-on:click="newGame"
        >Start New Game</b-button
      >
    </b-card>
    <b-alert :show="gameEnded" variant="danger">{{ winner }}</b-alert>
    <b-card class="mb-2 text-center d-flex">
      <div class="turn-list">
        <ul>
          <li v-for="(turn, index) in turns" :key="index">
            {{ turn }}
          </li>
        </ul>
      </div>
    </b-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      playerHealth: 200,
      monsterHealth: 200,
      turns: [],
      gameIsRunning: false,
      gameEnded: false,
      gaveUp: false,
      winner: "",
    };
  },
  methods: {
    heal: function() {
      if (this.playerHealth < 190) {
        this.playerHealth += 10;
        let attackValue = Math.floor(Math.random() * 15) + 1;
        this.playerHealth -= attackValue;
        this.turns.unshift(
          `Player healed 10 health   |   Monster attacked for ${attackValue} damage!`
        );
        this.checkWinner();
      }
    },
    attack: function() {
      this.monsterHealth -= 5;
      let attackValue = Math.floor(Math.random() * 15) + 1;
      this.playerHealth -= attackValue;
      this.turns.unshift(
        `Player attacked for 5 damage   |   Monster attacked for ${attackValue} damage!`
      );
      this.checkWinner();
    },
    specialAttack: function() {
      this.monsterHealth -= 15;
      let attackValue = Math.floor(Math.random() * 15) + 1;
      this.playerHealth -= attackValue;
      this.turns.unshift(
        `Player used SPECIAL attack for 15 damage   |   Monster attacked for ${attackValue} damage!`
      );
      this.checkWinner();
    },
    newGame: function() {
      this.turns = [];
      this.playerHealth = 200;
      this.monsterHealth = 200;
      this.gameEnded = false;
    },
    checkWinner: function() {
      if (this.playerHealth <= 0) {
        this.winner = "The winner is Monster!!";
        this.gameEnded = true;
      }
      if (this.monsterHealth <= 0) {
        this.winner = "You are the winner!! Congratulations!";
        this.gameEnded = true;
      }
    },
    giveUp: function() {
      this.winner = "Pathetic. I excpected more from you.";
      this.gameEnded = true;
    },
  },
};
</script>

<style scoped>



.container {
  max-width: 900px;
  width: 90%;
}
.player {
  width: 50%;
  color:white;
}

.health-bar {
  border-radius: 50px;
  position: relative;
  width: 90%;
  height: 40px;
  background-color: rgb(219, 219, 219);
}
.health {
  color:black;
  position: relative;
  z-index: 100;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.health-empty {
  height: 40px;
  background-color: transparent;
}

/* ACTION BUTTONS */
.attack-button:hover,
.heal-button:hover,
.give-up-button:hover,
.new-game-button {
  transform: scale(1.1);
}
.special-attack-button:hover {
  transform: scale(1.3);
}
.mr-2 {
  transition: transform 0.5s;
}
.mb-2{
  background:#7f7f7f;
  background:rgba(0,0,0,0.3);
}

/* TURN LIST */

.turn-list li {
  padding: 10px;
  list-style: none;
  border-bottom: 3px solid red;
  background-color: lightgrey;
  margin-right:35px;
  margin-bottom:10px;
  border-radius: 10px;
  font-weight: bold;

}

.bootstrap-health {
  height: 100%;
}
</style>
