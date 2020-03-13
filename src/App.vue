<template>
  <div id="app">
    <v-table :data="sample"></v-table>
  </div>
</template>

<script>
import VTable from "./components/VTable.vue";
import sample from "../sample.json";

export default {
  name: "App",
  data () {
    return {
      sample
    };
  },
  components: {
    VTable
  }
};
</script>

<style lang="scss">
html,
body {
  padding: 0;
  margin: 0;
}
</style>
