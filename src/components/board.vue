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
            v-on:gameStarted="gameStarted=true" 
            :map="bombMap">
            </MineButton>
            </div>
            </div>
    </div>
</template>

<script>
import MineButton from './mine-button.vue'
import ResetButton from './reset-button.vue'
import NumberBox from './number-box.vue'

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
      }
    },
    props: {
           
    },
    computed: {},
    mounted() {
       
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
            console.log("creating bomb map") 
            let map = [];
            for (let row = 0; row < this.selectedSize; row++) {
              
            for (let col = 0; col < this.selectedSize; col++) {
                map.push({row: row, col: col, bomb: true})
            }
            }
               console.log("map", map)
               this.bombMap = map;
        }
    },
     watch: {
         gameStarted() {
             if (this.gameStarted) {
             console.log("started")
             this.createBombMap();
             this.startTimer();
             }
         },
         boardReset() {
              console.log("stopped")
             this.stopTimer();
             this.timer = 0;
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
