<template>
  <div id="app">
    <div class="status">{{ statusText }}</div>
   
    <button class="reset-btn" @click="reset">Reset</button>
  
    <div v-for="row in 3" :key="'row-' + row">
      <div class="row">
        <button v-for="button in 3"  
          @click="clickBox(row, button)" 
          :disabled="winner"
          :key="'row-' + row + '-col-' + button"
          class="col square">
          {{ boxes[(row - 1) * 3 + (button - 1)]}}
        </button>
      </div>
    </div>
    
  </div>
</template>

<script>
  export default {
    name: "App",
    data() {
      return {
        winner: null,
        currentPlayer: 'X',
        boxes: Array(9).fill("")
      };
    },
    computed: {
      statusText() {
        if (this.winner) {
          return `Winner: ${this.winner}`;
        }
        if (this.isDraw) {
          return "It's a draw";
        }
        return `Next player: ${this.currentPlayer}`;
      },
      isDraw() {
        return !this.winner && this.boxes.every(box => box !== "");
      },
    },
    mounted () {
      

    },
    methods: {
      clickBox (row,button) {

        const index = (row - 1) * 3 + (button - 1);
       
        if (this.boxes[index] || this.winner || this.isDraw) {
          return;
        }
     
        this.$set(this.boxes, index, this.currentPlayer);
     
        const winner = this.checkWinner();
        
        if (winner) {
          this.winner = winner;
        } else {
          this.currentPlayer = (this.currentPlayer === 'X') ? 'O' : 'X'
        }

      },
      reset () {
        this.winner = null
        this.currentPlayer = 'X'
        this.boxes = Array(9).fill("")
      },
      checkWinner () {
        const combos = [
          [0, 1, 2],
          [3, 4, 5],
          [6, 7, 8],
          [0, 3, 6],
          [1, 4, 7],
          [2, 5, 8],
          [0, 4, 8],
          [2, 4, 6],
        ];

        for (const [a, b, c] of combos) {
          if (
            this.boxes[a] &&
            this.boxes[a] === this.boxes[b] &&
            this.boxes[a] === this.boxes[c]
          ) {
            return this.boxes[a];
          }
        }

        return null;
        /*return combos.some(([a,b,c]) => {
          return (
           this.boxes[a] &&
           this.boxes[a] === this.boxes[b] && 
           this.boxes[a] === this.boxes[c]
          )
        })*/
      }
    }
  };
</script>
<style>
.status{
  margin-bottom:10px;
}
.square{
  width: 70px;
  height: 70px;
  margin: 10px;
  padding: 27px;
}
.reset-btn{
  margin-top: 20px;
  margin-bottom: 20px;
}
</style>