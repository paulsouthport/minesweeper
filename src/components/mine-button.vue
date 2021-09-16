<template>
  <div tabindex="0" 
          class='mine-button' 
          :class='{ "mouse-down": (mouseDown||clicked), "flagged": rightClicked, "bomb": (isBomb && clicked) }'
          @click="onClicked"   
          @mousedown.prevent="onMouseDown" 
          @mouseleave="blurred" 
          @mouseenter="mouseEnter"
          @contextmenu="onRightClicked">
  <!-- {{isBomb ? 'X': ''}} -->
  </div>
</template>

<script>
import { STATES } from '../enums';

  export default {
    name: 'mine-button',
    props: {
        position: Object,
        reset: Number,
        isBomb: Boolean,
       
    },
    data() {
      return {
        clicked: false,
        rightClicked: false,
        mouseDown: false,
        
      }
    },
    computed: {
      
    },
    mounted() {},
    methods: {
      blurred() {
       this.mouseDown = false;
      },
      mouseEnter(e) {
       this.mouseDown = e.buttons == 1;
      },
      onMouseDown(e) {
        this.mouseDown = e.buttons == 1;
      },
      onRightClicked(e) {
         if (this.rightClicked) {
           this.rightClicked = false
         }
         else {
          this.rightClicked = this.clicked == false ? true : false;
          this.$emit('game-started');
        }
        e.preventDefault();
      },
      onClicked() {
        this.rightClicked = false;
        this.clicked = true;
        this.$emit('game-started');
      },
     
    },
    watch: {
      reset() {
       
        this.clicked = false;
        this.rightClicked = false;
        this.mouseDown = false;
        },
       
        clicked() {
            this.$emit("state", this.position, this.clicked ? STATES.DOWN : STATES.UP, this.isBomb);
        },
        rightClicked() {
            this.$emit("state", this.position, this.rightClicked ? STATES.FLAGGED : STATES.UP, false);
        }
      
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>


.mine-button {
   height: 25px;
    width: 25px;
    background-color: #C0C0C0;
    border: 3px solid;
    border-top-color: #ffffff;
    border-right-color: #7B7B7B;
    border-bottom-color: #7B7B7B;
    border-left-color: #ffffff;
    text-align: center;
    vertical-align: middle;
    box-sizing: border-box;
}

.mine-button.mouse-down {
     border: none;
}

.mine-button.flagged {
  background-image: url('~@/assets/flag.png');
    background-size: contain;
}

.mine-button.bomb {
  background-image: url('~@/assets/bomb.png');
    background-size: contain;
    background-color: red;
}
</style>