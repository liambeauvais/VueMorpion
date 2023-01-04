<template>
  <div class="morpion">
    <h1>{{ msg }}</h1>
    <p>Les règles sont simples: le jeu se joue à deux, les joueurs devant "placer" un pion de sa couleur l'un après
      l'autre.</p>
    <p>Le gagnant est celui qui arrive à aligner 3 de ses jetons horizontalement, verticalement ou en diagonale.</p>
    <h2 v-if="players.player_1.name">Bienvenue {{ player_1.name }} (joue avec X)</h2>
    <input v-if="!players.player_1.name" @keyup.enter="definePlayerName(0)" v-model="player_1.name"
           placeholder="Nom du joueur 1">
    <hr>
    <h2 v-if="players.player_2.name">Bienvenue {{ player_2.name }} (joue avec O)</h2>
    <input v-if="!players.player_2.name" @keyup.enter="definePlayerName(1)" v-model="player_2.name"
           placeholder="Nom du joueur 2">

    <p class="player-turn" v-if="playersFilled">
    <span v-if="player_turn">À {{players.player_1.name}} de jouer!</span>
    <span v-if="!player_turn">À  {{players.player_2.name}} de jouer!</span>
    </p>
    <div v-if="playersFilled" class="morpion-square">
      <MorpionTile class="morpion-tile" v-for="(tile,index) of tiles" :key="index" @click="changeValue(index)"
                   :value="tile"></MorpionTile>
    </div>
  </div>
  <win-modal v-on:close="replay" :ties="tieGamesCount" :players="players" :winner="winner" v-if="gameIsWon"></win-modal>
</template>

<script>
import MorpionTile from "@/components/MorpionTile";
import WinModal from "@/components/winModal";

export default {
  name: 'MorpionGame',
  components: {
    WinModal,
    MorpionTile,
  },

  props: {
    msg: String
  },
  data() {
    return {
      tieGamesCount: 0,
      playersFilled: false,
      gameIsWon: false,
      tiles: [null, null, null, null, null, null, null, null, null],
      winner: null,
      player_turn: true,
      player_1: {
        name: null,
      },
      player_2: {
        name: null,
      },
      players: {
        player_1: {
          name: null,
          points: 0,
          symbol: "X"
        },
        player_2: {
          name: null,
          points: 0,
          symbol: "O"
        },
      },
    }
  },
  methods: {
    changeValue(index) {
      if (!this.tiles[index]) {
        this.tiles[index] = this.player_turn ? this.players.player_1.symbol : this.players.player_2.symbol;
        this.isWin();
        this.player_turn = !this.player_turn;
      }
    },
    definePlayerName(index) {
      if (index === 0) {
        this.players.player_1.name = this.player_1.name;
      } else {
        this.players.player_2.name = this.player_2.name;
      }
      if (this.players.player_1.name && this.players.player_2.name) {
        this.playersFilled = true;
      }
    },
    isWin() {
      if (((this.tiles[0] === this.tiles[1] && this.tiles[1] === this.tiles[2]) && this.tiles[0])
          || ((this.tiles[0] === this.tiles[3] && this.tiles[3] === this.tiles[6]) && this.tiles[0])
          || ((this.tiles[0] === this.tiles[4] && this.tiles[4] === this.tiles[8]) && this.tiles[0])
          || ((this.tiles[1] === this.tiles[4] && this.tiles[4] === this.tiles[7]) && this.tiles[1])
          || ((this.tiles[2] === this.tiles[5] && this.tiles[5] === this.tiles[8]) && this.tiles[2])
          || ((this.tiles[2] === this.tiles[4] && this.tiles[4] === this.tiles[6]) && this.tiles[2])
          || ((this.tiles[3] === this.tiles[4] && this.tiles[4] === this.tiles[5]) && this.tiles[3])
          || ((this.tiles[6] === this.tiles[7] && this.tiles[7] === this.tiles[8]) && this.tiles[6])
      ) {
        this.winner = this.player_turn ? this.players.player_1.name : this.players.player_2.name;
        if (this.player_turn) {
          this.players.player_1.points++;
        } else {
          this.players.player_2.points++;
        }
        this.gameIsWon = true;
      }
      else{
        this.isTie();
      }
    },
    isTie() {
      let onlyFilledTiles = true;
      for (let tile of this.tiles) {
        if (!tile) {
          onlyFilledTiles = false;
        }
      }
      if (onlyFilledTiles) {
        this.tieGamesCount++;
        this.replay();
      }
    },
    replay() {
      this.gameIsWon = false;
      this.tiles = [null, null, null, null, null, null, null, null, null];
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.morpion {
  display: flex;
  flex-flow: wrap column;
  align-items: center;
  justify-content: start;
}

h3 {
  margin: 40px 0 0;
}

hr {
  width: 50%;
  margin: auto;
}

input {
  width: 200px;
  padding: 10px;
  margin: 2px;
}

.morpion-square {
  margin: 20px auto auto;
  display: flex;
  flex-flow: row wrap;
  justify-content: start;
  width: 604.8px;
  height: 604.8px;
}

.morpion-tile {
  width: 200px;
  height: 200px;
}

h1,p{
  text-align: center;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.player-turn{
  font-size: 20px;
  font-weight: bolder;
  font-style: italic;
}




@media (max-width: 650px){
  .morpion-square{
    width: 304.8px;
    height: 304.8px;
  }
  .morpion-tile{
    width: 100px;
    height: 100px;
  }
}
</style>
