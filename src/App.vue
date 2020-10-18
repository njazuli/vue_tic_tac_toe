<template>
  <div id="app">
    <div class="container">
      <div class="row my-2" v-if="status">
        <div class="col-12">
          <div class="w-100 d-flex justify-content-center">
            <div class="alert alert-success col-8" role="alert">
              <h2>Player {{ player }} wins! </h2>
              <p class="mb-0"> Click 'Reset' button to restart </p>
            </div>
          </div>
        </div>
      </div>
      <div class="row justify-content-center">
        <div class="col-12">
          <h2>TIC TAC TOE - FROM NJ</h2>
        </div>
        <div class="col-12 my-3" v-if="playersTurnDiv">
          <div class="w-100 d-flex justify-content-center">
            <div class="alert alert-secondary" role="alert">
              Next player: {{ player }}
            </div>
          </div>
        </div>
        <div class="col-12 mt-3">
          <div class="w-100 d-flex justify-content-center">
            <div class="board_container" id="board">
              <div class="row">
                <div v-for="(button,index) in buttons" :key="'btn'+index" :id="'cell'+index" class="col-4 border m-0 p-0 cell" :data-cell="index" @click="handleCellClick(index,$event)">
                  <div class="w-100 h-100 d-flex align-items-center justify-content-center cell_in">
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="row my-3">
        <div class="col-12">
          <div class="btn btn-primary" @click="reset()">
            Reset
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// 1) set data variables
// player -> set player to determine the current player
// active -> check if game is finished
// gamestate -> t0 check current turn
// winningconditions -> cell combinations of winning state


// guidelines from :
// https://medium.com/javascript-in-plain-english/build-tic-tac-toe-game-using-javascript-3afba3c8fdcc
export default {
  name: 'App',
  data() {
    return {
      buttons: 9,
      active: true,
      player: 'X',
      playersTurnDiv: true,
      status: false,
      gameState: ["", "", "", "", "", "", "", "", ""],
      winningCombinations : [
          [0, 1, 2],
          [3, 4, 5],
          [6, 7, 8],
          [0, 3, 6],
          [1, 4, 7],
          [2, 5, 8],
          [0, 4, 8],
          [2, 4, 6]
      ]
    }
  },
  methods:{
    handleCellClick(value,e) {

      // console.log(value);
      
      // check if the specific cell is already clicked and game is ongoing,
      // if yes,ignore
      // else, store the cell into gameState,and display the current's players in the selected cell

      const clickedCell = e.target;

      if(this.gameState[value] != "" || !this.active){
        return
      }

        
      this.handleCellPlayed(value,clickedCell);
      this.handleResultValidation();
      
    },
    handleCellPlayed(currentcell,cell) {
      //insert into gamestate arr
      this.gameState[currentcell] = this.player;
      cell.innerHTML = this.player;
    },

    handleResultValidation() {
      // first, set a variable as false,and if the checking has detected that there is same combinations of winning state,then the variable will be set as true
      // second, do the checking of the winning combination
      // third, check if the game is draw
      let roundWon = false;
      for (let i = 0; i <= 7; i++) {
        const winCondition = this.winningCombinations[i];
        let a = this.gameState[winCondition[0]];
        let b = this.gameState[winCondition[1]];
        let c = this.gameState[winCondition[2]];
        if (a === '' || b === '' || c === '') {
            continue;
        }
        if (a === b && b === c) {
            roundWon = true;
            break
        }
      }

      if (roundWon) {
        this.status = true;
        this.active = false;
        this.playersTurnDiv = false;
        return;
      }

      // check if game state is not filled with any numbers, if no empty slot in game state array,meaning that the game is draw
      // set a variable starting with false
      // if the variable true,then set the active variable as false
      let roundDraw = !this.gameState.includes("");
      if (roundDraw) {
          this.active = false;
          this.playersTurnDiv = false;
          return;
      }
     
      this.handlePlayerChange();
    },

    handlePlayerChange() {
      this.player = this.player === "X" ? "O" : "X";
    },

    reset(){
      this.active = true;
      this.player = 'X';
      this.playersTurnDiv = true;
      this.status = false;
      this.gameState = ["", "", "", "", "", "", "", "", ""];
      document.querySelectorAll('.cell_in').forEach(cell => cell.innerHTML = "");
    }
  }
}
</script>

<style>
#app {
  font-family: 'Lato', sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.board_container{
  width: 300px;
}
.col-4{
  height: 100px;
}
.cell:hover{
  background: #cccccc;
}
</style>
