<template>
    <div class="board" >
        <select name="size" v-model.number=selectedSize>
           <option v-for="option in selectOptions" :key=option>{{option}}</option>
        </select>
        {{boardReset}}
<div class="controls">
    <div>C</div>
<ResetButton v-on:reset="reset"></ResetButton>
<div>T</div>

</div>
  
       <div v-for="row in selectedSize" :key=row class="board-rows">
           <div class="row">
            <MineButton v-for="col in selectedSize" :key=col :position="[row, col]" :reset="boardReset"></MineButton>
            </div>
            </div>
    </div>
</template>

<script>
import MineButton from './mine-button.vue'
import ResetButton from './reset-button.vue'

export default {
    name: 'board',
    components: {
        MineButton,
        ResetButton
  },
     data() {
      return {
        selectOptions: [5, 10, 15, 25],
        selectedSize: 5,
        boardReset: 0,
      }
    },
    props: {
           
    },
    computed: {},
    mounted() {},
    methods: {
        reset() {
           this.boardReset++;
         
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
}
</style>
