 //TODO: find all tiles neighbouring bombs && counts
 //TODO: spread clear on safe clicked
 //TODO: bevelled/raised outer of board
 //TODO: Win 3.11 style form header
 //TODO: forat negative counter better, find LCD style monospace web font
 //TODO: handle reset, remap bombs/new map on grid resize, or prevent resize mid game 
 //TODO: grid lines around tiles with clicked neighbours
 //TODO: replace y keyboard because n & m keys do't work 50% of the tie. Thaks Scarlett!

<template>
    <div class="board" >
        <div class="controls text">
            <div>Grid Size: 
               <select name="size" v-model.number=selectedSize>
           <option v-for="option in selectOptions" :key=option>{{option}}</option>
        </select> 
            </div>
        
         Tries: {{boardReset}}
        </div>
       
<div class="controls numbers text">
    <NumberBox id="counter" :value="counter"></NumberBox>
    <ResetButton v-on:reset="reset"></ResetButton>
    <NumberBox id="timer" :value="timer"></NumberBox>

</div>
  
       <div v-for="row in selectedSize" :key=row class="board-rows">
           <div class="row">
            <MineButton v-for="col in selectedSize" 
            :key=col 
            :position="{row: row, col: col}" 
            :reset="boardReset" 
            :isBomb="isBomb(row, col)"
            v-on:gameStarted="gameStarted=true" 
            v-on:state="stateChanged"
            >
            </MineButton>
            </div>
            </div>
    </div>
</template>

<script>
import MineButton from './mine-button.vue'
import ResetButton from './reset-button.vue'
import NumberBox from './number-box.vue'

import { STATES } from '../enums';

export default {
    name: 'board',
    components: {
        MineButton,
        ResetButton,
        NumberBox,
  },
     data() {
      return {
        selectOptions: [9, 12, 18],
        selectedSize: 9,
        boardReset: 1,
        counter: 0,
        timer: 0,
        clock: '',
        gameStarted: false,
        bombMap: [],
        hitMine: false,
      }
    },
    props: {
           
    },
    computed: {},
    mounted() {
        this.createBombMap();
    },
    methods: {
        reset() {
           this.boardReset++;
         
        },
        startTimer() {
            this.clock = setInterval(this.incrementTimer, 1000);    
        },
        stopTimer() {
            clearInterval(this.clock);    
            this.gameStarted = false;
        },
        incrementTimer() {
            this.timer++;
        },
        createBombMap() {
           
            let map = [];
            for (let row = 1; row < this.selectedSize + 1; row++) {
              
                for (let col = 1; col < this.selectedSize + 1; col++) {
                    var x = Math.floor((Math.random() * 5) + 1);
                    map.push({row: row, col: col, bomb: x == 1, state: STATES.UP})
                }
            }              
            this.bombMap = map;
            this.counter = map.filter(o => o.bomb == true).length;
        },
        stateChanged(position, state, bomb) {
            let mineHit = state == STATES.DOWN && bomb;
           // console.log(position, state, bomb, mineHit ? "hit a mine": '')
           if (state == STATES.FLAGGED) {this.counter--}
           if(state == STATES.UNFLAGGED) {this.counter ++}
            this.hitMine = mineHit;
        },
        isBomb(row, col) {
            let bomb = this.bombMap.find(o => {
                    return o.row == row && o.col == col
                })?.bomb;
          
          return bomb;
        }
    },
     watch: {
         gameStarted() {
             if (this.gameStarted) {
             console.log("started")
             this.startTimer();
             } 
         },
         boardReset() {
             console.log("stopped")
             this.stopTimer();
             this.timer = 0;
             this.createBombMap();
             this.hitMine = false;
         },
         hitMine() {
             this.gameStarted = this.hitMine ? false : true; 
               this.stopTimer();
             console.log("here", this.gameStarted)
         }
    }
    
  }
</script>


<style>

.board {
    background-color: #C0C0C0;
    padding: 35px;
    display: inline-block;
    min-width: 100px;
    margin: auto;
}
.row {
    display: flex;
    flex-direction: row;
    
}

.controls {
    padding: 10px 0px 10px 0px;
    display: flex;
    width: 100%;
    flex-direction: row;
    justify-content: space-between;
    font-weight: bold;
}

.controls.numbers {
    font-size: xx-large;
}
</style>
