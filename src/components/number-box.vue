<template>
  <div class="container">
    {{paddedValue}}
  </div>
 
</template>

<script>
  export default {
    name: 'number-box',
    props: {
        value: Number,
    },
    data() {
      return {
      
        
      }
    },
    computed: {
      paddedValue() {
        let num = this.value.toString();
        while (num.length < 3 && num >= 0) num = "0" + num;
        return num;
      }
     
    },
    mounted() {

    },
       
    watch: {
     
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.container {
  background-color: black;
  color: red;
  width: 85px;
  height: 40px;
  border:1px solid darkgrey;
}
    
</style>