<template>
  <div id="app">
    <img src="../static/TR-Logo.png" class="tr-logo"/>
    <div class="top">
      <h4 v-text="instructions"></h4>
      <ol>
        <li v-for="item in items">{{ item }}</li>
      </ol>
    </div>
    <puzzle/>
  </div>
</template>

<script>
import Puzzle from './components/Puzzle'

export default {
  name: 'app',
  components: {
    Puzzle
  },
  data() {
    return {
      instructions: 'Rearrange the pieces to complete the puzzle!',
      items: [
        'Drag and drop the pieces one at a time',
        'Try to match each piece to the designer\'s name in as few guesses as possible!'
      ]
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px 0;
}
.top {
  width: 300px;
  margin: 0 auto 30px auto;
}
.tr-logo {
  width: 100px;
  height: 96px;
}
.top ol {
  text-align: left;
}
</style>
