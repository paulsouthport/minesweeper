<template>
  <div tabindex="0" 
          class='mine-button' 
          :class='{ "mouse-down": (mouseDown||clicked), "flagged": rightClicked }'
          @click="onClicked"   
          @mousedown.prevent="onMouseDown" 
          @mouseleave="blurred" 
          @mouseenter="mouseEnter"
          @contextmenu="onRightClicked">

  </div>
</template>

<script>
  export default {
    name: 'mine-button',
    props: {
        position: Array,
        reset: Number,
    },
    data() {
      return {
        clicked: false,
        rightClicked: false,
        mouseDown: false,
        
      }
    },
    computed: {
      buttonClicked() {
          let butt = "";
          this.clicked ? butt = "L" : "";
          this.rightClicked ? butt = "R" : butt;
          this.clicked != this.rightClicked;
          if(this.clicked || this.rightClicked) {this.$emit('game-started')}
          return butt;
      } 
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
      } 
    },
    watch: {
      reset() {
       
        this.clicked = false;
        this.rightClicked = false;
        this.mouseDown = false;
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
</style>