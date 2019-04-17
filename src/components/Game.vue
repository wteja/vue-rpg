<template>
  <div id="game">
    <h1 v-if="gameOver">Game Over</h1>
    <div class="header">
      <div class="actor player">
        <div class="name">Player</div>
        <div class="hp-container">
          <div class="hp-value" :style="{ width: playerHp + '%' }"></div>
          <div class="hp-percent">{{playerHp}}%</div>
        </div>
      </div>
      <div class="actor monster">
        <div class="name">Monster</div>
        <div class="hp-container">
          <div class="hp-value" :style="{ width: monsterHp + '%' }"></div>
          <div class="hp-percent">{{monsterHp}}%</div>
        </div>
      </div>
    </div>
    <div v-if="!startGame">
      <button class="button" @click="doStartGame">Start Game</button>
    </div>
    <div v-if="startGame">
      <button class="button" @click="attack">Attack</button>
      <button class="button" @click="heal">Heal</button>
      <button class="button" @click="giveUp">Give Up</button>
    </div>
    <div class="result">
      <div v-for="(log, i) in logs" :key="i">{{log}}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Game",
  data() {
    return {
      playerHp: 100,
      monsterHp: 100,
      startGame: false,
      gameOver: false,
      logs: []
    };
  },
  methods: {
    doStartGame() {
      this.startGame = true;
      this.gameOver = false;
      this.logs = [];
      this.playerHp = 100;
      this.monsterHp = 100;
    },
    attack() {
      if (this.monsterHp <= 0) return;

      this.monsterHp = this.monsterHp - 10;
      if (this.monsterHp < 0) {
        this.monsterHp = 0;
      }
      this.playerHp = this.playerHp - 15;
      if (this.playerHp < 0) {
        this.playerHp = 0;
        this.gameOver = true;
        this.startGame = false;
      }

      this.logs.push("Player attack monster 10 dmg.");
      this.logs.push("Monster attack player 15 dmg.");
    },
    giveUp() {
      this.startGame = false;
      this.gameOver = false;
      this.playerHp = 100;
      this.monsterHp = 100;
    },
    heal() {
      this.playerHp += 30;
      if (this.playerHp > 100) {
        this.playerHp = 100;
      }
      this.logs.push("Player heal 30 hp.");
    }
  }
};
</script>

<style lang="scss" scoped>
#game {
  width: 1200px;
  margin: 0 auto;

  .header {
    .player {
      float: left;
      width: 50%;
    }
    .monster {
      float: right;
      width: 50%;
    }

    .actor {
      .hp-container {
        width: 300px;
        height: 60px;
        border: 2px solid green;
        margin: 20px auto;
        position: relative;
      }

      .hp-value {
        background-color: green;
        width: 100%;
        height: 100%;
        transition: 0.3s ease width;
      }

      .hp-percent {
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        z-index: 2;
        color: #000;
      }
    }
  }

  .button {
    padding: 30px;
    border: 2px solid #000;
    background-color: #fff;
    outline: none;
    margin-bottom: 30px;

    &:hover {
      color: #fff;
      background-color: #000;
    }

    & + .button {
      margin-left: 20px;
    }
  }

  .result {
    border: 2px solid #000;
    width: 100%;
    min-height: 300px;
  }
}
</style>
