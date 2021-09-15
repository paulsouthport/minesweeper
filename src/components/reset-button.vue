<template>
  <div tabindex="0" 
          class='reset-button' 
          :class='{ "mouse-down": mouseDown }'
          @click="onClicked"   
          @mousedown.prevent="onMouseDown" 
         
          @contextmenu="onRightClicked">
        

  </div>
</template>

<script>
  export default {
    name: 'reset-button',
    props: {
       
    },
    data() {
      return {
              mouseDown: false,
       
      }
    },
    computed: {
     
    },
    mounted() {},
    methods: {
      
        onMouseDown(e) {
        this.mouseDown = e.buttons == 1;
      },
     
      onClicked() {
        this.mouseDown = false;
        this.$emit('reset');
      } ,
      onRightClicked(e) {
       
        e.preventDefault();
      },
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>


.reset-button {
   height: 40px;
    width: 40px;
    background-color: #C0C0C0;
    border: 3px solid;
    border-top-color: #ffffff;
    border-right-color: #7B7B7B;
    border-bottom-color: #7B7B7B;
    border-left-color: #ffffff;
    text-align: center;
    vertical-align: middle;
    box-sizing: border-box;
    background-image: url('~@/assets/smiley.png');
    background-size: contain;
}

.reset-button.mouse-down {
 
    border-bottom: 1px solid #7B7B7B;
    border-left-color: #7B7B7B;
    border-top-color: #7B7B7B;
    border-right: 1px solid #7B7B7B;

}
</style>