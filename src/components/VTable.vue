<template>
  <table v-if="data[0]" ref="table" class="v-table">
    <thead>
      <tr>
        <th v-for="(t, i) in Object.keys(data[0])"
            :key="i"
            ref="th">
            {{t}}
            <span class="resize-handle"></span>
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
export default {
  name: "VTable",
  props: {
    data: {
      default: false
    },
    COLUMN_MIN: {
      type: Number,
      default: 50
    },
    HANDLE_WIDTH: {
      type: String,
      default: "10px"
    },
    LAST_CELL_MAX: {
      type: String,
      default: "auto"
    }
  },
  data () {
    return {
      columns: [],
      headerBeingResized: ""
    };
  },
  methods: {
    initResize ({ target }) {
      this.headerBeingResized = target.parentNode;
      window.addEventListener("mousemove", this.onMouseMove);
      window.addEventListener("mouseup", this.onMouseUp);
      this.headerBeingResized.classList.add("header--being-resized");
    },
    onMouseMove (e) {
      requestAnimationFrame(() => {
        const horizontalScrollOffset = document.documentElement.scrollLeft;
        const width = (horizontalScrollOffset + e.clientX) - this.headerBeingResized.offsetLeft;

        const column = this.columns.find(({ header }) => header === this.headerBeingResized);
        column.size = Math.max(this.COLUMN_MIN, width) + "px";

        this.columns.forEach((column) => {
          if (column.size.startsWith("minmax")) {
            column.size = parseInt(column.header.clientWidth, 10) + "px";
          }
        });
        const newGrindTemplateColumns = this.columns
          .map(({ header, size }, index) => size);
        newGrindTemplateColumns[newGrindTemplateColumns.length - 1] =
            `minmax(${newGrindTemplateColumns[newGrindTemplateColumns.length - 1]}, ${this.LAST_CELL_MAX})`;
        this.$refs.table.style.gridTemplateColumns = newGrindTemplateColumns.join(" ");
      });
    },
    onMouseUp (e) {
      window.removeEventListener("mousemove", this.onMouseMove);
      window.removeEventListener("mouseup", this.onMouseUp);

      this.headerBeingResized.classList.remove("header--being-resized");
      this.headerBeingResized = null;
    }
  },
  mounted () {
    const max = "100fr";
    this.$el.querySelectorAll("th").forEach((header) => {
      this.columns.push({ header, size: `minmax(${this.COLUMN_MIN}px, ${max})` });
      const handle = header.querySelector(".resize-handle");
      handle.addEventListener("mousedown", this.initResize);
      handle.style.width = this.HANDLE_WIDTH;
    });
  }
};
</script>

<style scoped lang="scss">
// ++++++++++++++++ gloabal ++++++++++++++++++
* {
  box-sizing: border-box;
}
html,
body {
  padding: 0;
  margin: 0;
}
// ++++++++++++++++++ v2 ++++++++++++++
table {
  min-width: 100vw;
  width: auto;
  flex: 1;
  display: grid;
  border-collapse: collapse;
  /* These are just initial values which are overriden
  using JavaScript when a column is resized */
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
  user-select: none;
}

th:last-child {
  border: 0;
  background-color: yellowgreen;
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
/* The following selector is needed so the handle is visible during
resize even if the mouse isn't over the handle anymore */
.header--being-resized .resize-handle {
  opacity: 0.5;
}

.header--being-resized {
  background-color: yellow;
}

th:hover .resize-handle {
  opacity: 0.3;
}
</style>
