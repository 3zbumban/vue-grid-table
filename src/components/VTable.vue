<template>
  <table v-if="data[0]" ref="table" class="v-table">
    <thead>
      <tr>
        <th v-for="(t, i) in Object.keys(data[0])"
            :key="i"
            ref="th">
            {{t}} <span class="resize-handle"></span>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(o, i) in data" :key="i">
        <td v-for="(e, i) in o" :key="i">{{e}}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
// 0: min value
// eslint-disable-next-line no-unused-vars
const min = 150;

export default {
  name: "VTable",
  props: ["data"],
  data () {
    return {
      // 1: save table element
      table: this.$refs.table,
      columns: [],
      headerBeingResized: ""
    };
  },
  mounted () {
    console.log(this.$refs.th);
  }
};
</script>

<style scoped lang="scss">
* {
  box-sizing: border-box;
}

html,
body {
  padding: 0;
  margin: 0;
}

// body {
//   font-family: BlinkMacSystemFont, -apple-system, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", "Helvetica", "Arial", sans-serif;
// }

// version 1 ++++++++++++++++++++++++++++++++++++++
// table {
//   display: grid;
//   border-collapse: collapse;
//   min-width: 100%;
//   grid-template-columns:
//     minmax(150px, 1fr)
//     minmax(150px, 1.67fr)
//     minmax(150px, 1.67fr)
//     minmax(150px, 1.67fr)
//     minmax(150px, 3.33fr)
//     minmax(150px, 1.67fr)
//     minmax(150px, 3.33fr)
//     minmax(150px, 1.67fr);
// }
// ++++++++++++++++++++++++++++++++++++++++++++++

// version 2 ++++++++++++++++++++++++++++++++++++
table {
  min-width: 100vw;
  width: auto;
  flex: 1;
  display: grid;
  border-collapse: collapse;
  /* These are just initial values which are overriden using JavaScript when a column is resized */
  grid-template-columns:
    minmax(150px, 1fr)
    minmax(150px, 1.67fr)
    minmax(150px, 1.67fr)
    minmax(150px, 1.67fr)
    minmax(150px, 3.33fr)
    minmax(150px, 1.67fr)
    minmax(150px, 3.33fr)
    minmax(150px, 1.67fr);
}
// ++++++++++++++++++++++++++++++++++++++++++++++

thead,
tbody,
tr {
  display: contents;
}

th,
td {
  padding: 15px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

th {
  position: sticky;
  top: 0;
  background: #6c7ae0;
  text-align: left;
  font-weight: normal;
  font-size: 1.1rem;
  color: white;
}

th:last-child {
  border: 0;
}

td {
  padding-top: 10px;
  padding-bottom: 10px;
  color: #808080;
}

tr:nth-child(even) td {
  background: #f8f6ff;
}

// part 2 css  +++++++++++++++++++++++++++++++++++++++++++++++++
// resize handle css
.resize-handle {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  background: black;
  opacity: 0;
  width: 3px;
  cursor: e-resize;
}

.resize-handle:hover,
/* The following selector is needed so the handle is visible during resize even if the mouse isn't over the handle anymore */
.header--being-resized .resize-handle {
  opacity: 0.5;
}

th:hover .resize-handle {
  opacity: 0.3;
}

td {
  padding-top: 10px;
  padding-bottom: 10px;
  color: #808080;
}
</style>
