<template>
<div class="game">

<Board></Board>
</div>

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Orbitron&display=swap" rel="stylesheet">
</template>


<script>
import Board from './components/board.vue'

export default {
  name: 'App',
  components: {
   Board,
  },
 
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
.game {
  display: flex;
  flex-direction: column;
  width: auto;
}

.text {
  font-family: 'Orbitron', sans-serif;
     
    }

</style>
