<template>
  <div id="app">
    <img src="../static/TR-Logo.png" class="tr-logo"/>
    <div class="top">
      <h4 v-text="instructions"></h4>
      <ul>
        <li v-for="item in items">{{ item }}</li>
      </ul>
      <span class="bold">Hint:</span><span> The completed picture shows the TR logo!</span>
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
        '1.) Drag and drop the pieces one at a time',
        '2.) Try to match each piece to the designer\'s name in 7 guesses or less!'
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
  margin: 10px 0;
}
.top {
  margin: 0 auto 30px auto;
}
.top h4 {
  margin-bottom: 0;
}
.tr-logo {
  width: 100px;
  height: 96px;
}
.top ul {
  text-align: center;
  list-style: none;
  padding-left: 0;
  margin-top: 0;
}
.bold {
  font-weight: bold;
}
</style>
