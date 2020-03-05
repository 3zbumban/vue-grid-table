<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <v-table msg="Welcome to Your Vue.js App"></v-table>
  </div>
</template>

<script>
import VTable from "./components/VTable.vue"

export default {
  name: "App",
  components: {
    VTable
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
