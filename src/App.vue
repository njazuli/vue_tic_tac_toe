<template>
  <div id="app">
    <div class="container">
      <div class="row my-2">
        <div class="col-12">
          <div class="w-100 d-flex justify-content-center">
            <div class="alert alert-success" role="alert">
              Player {{ player }} wins! Click 'Reset' button to restart
            </div>
          </div>
        </div>
      </div>
      <div class="row justify-content-center">
        <div class="col-12">
          <h2>TIC TAC TOE - FROM NJ</h2>
        </div>
        <div class="col-12 my-3">
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
                  <div class="w-100 h-100 d-flex align-items-center justify-content-center">
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
      gameState: ["", "", "", "", "", "", "", "", ""],
      winningConditions : [
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
    reset(){
      console.log('testing');
    },
    handleCellClick(value,e) {

      // console.log(value);
      
      // check if the cell is already clicked and game is ongoing,
      // if yes,ignore
      // else, store the cell into gameState,and display the current's players in the selected cell

      const clickedCell = e.target;

      if(this.gameState[value] != "" && !this.active){
        return
      }else{
        this.handleCellPlayed(value,clickedCell);
        // this.handleResultValidation();
      }
    },
    handleCellPlayed(currentcell,cell) {
      //insert into gamestate arr
      this.gameState[currentcell] = this.player;
      cell.innerHTML = this.player;
    },
    handlePlayerChange() {

    },
    handleResultValidation() {

    },
    handleRestartGame() {

    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
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
