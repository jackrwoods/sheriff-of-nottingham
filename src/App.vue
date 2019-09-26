<!--
@Author: Jack Woods
@Date:   2019-09-24T13:48:04-07:00
@Email:  jackrwoods@gmail.com
@Filename: App.vue
@Last modified by:   Jack Woods
@Last modified time: 2019-09-24T14:04:47-07:00
-->

<template>
  <div id="app">
    <h1>Sheriff of Nottingham</h1>
    <CardCounter />
  </div>
</template>

<script>
import CardCounter from './components/CardCounter.vue'

export default {
  name: 'app',
  components: {
    CardCounter
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
  margin-top: 60px;
}
</style>
