# vue-grid-table

* https://github.com/3zbumban/vue-grid-table
* https://www.npmjs.com/package/vue-grid-table

## Installation

```bash
npm i vue-grid-table -S
```

```vue
<template>
    <div>
        <vue-grid-table :data="sampleData"></vue-grid-table>
    </div>
</template>

<script>
import vueGridTable from 'vue-grid-table'

export default {
  data () {
    return {
      sampleData // [{}, {}, {}, ...] 
      // this musst be an array with objects, 
      // the objekt keys are going to be the table headers 
      // and values are going to be the entries in the table
      // this is maybe not the best data structure 
      // and is maybe going to be changed in the future
    }
  },
  components: {
    vueGridTable
  }
}
</script>
```

### props:
  * data:
    * description: the data that should go into the table, object keys are getting used as table headers, values going into the table columns
    * type: Array of objects
    * notice: you have to take care that the objects in the array are uniform so the component cn work correctly
  * COLUMN_MIN:
    * description: minimum value a column can be resized to as a number (of pixels)
    * type: Number
    * notice: the number you provide is going to be seet to a pixel value
    * default: 50 -> ('50px')
  * HANDLE_WIDTH:
    * description: the width of the resize handle in pixels
    * type: String
    * notice: you have to provide a string value in this form `'10px'`
    * default: '10px'
  * LAST_CELL_MAX:
    * description: this is the value that gets set as the second argument for the `minmax(COLUMN_MIN, LAST_CELL_MAX)` of the last grid-template item
    * type: String
    * notice: this is just to make it possible to customize the behavior of the last column his prop might be unnesessary
    * default: 'auto'

### CSS

* Theese are (most liekely) the css selectors you need to customize your table.
* modifying these or espacially other css might brake the component 

```css
/* table headers*/
th {
  background: #6c7ae0; 
  text-align: left;
  font-weight: normal;
  font-size: 1.1rem;
  color: white;
  user-select: none;
}

/* last table header color*/
th:last-child {
  background-color: yellowgreen;
}

/* text color inside table */
td {
    color: #808080;
}

/* color pattern effect */
tr:nth-child(even) td {
  background: #f8f6ff;
}

.resize-handle {
  opacity: 0;
  cursor: e-resize;
}

/* resize handle while resizing */
.resize-handle:hover,
/* The following selector is needed so the handle is visible during
resize even if the mouse isn't over the handle anymore */
.header--being-resized .resize-handle {
  opacity: 0.5;
}

/* the one beeing resized now */
.header--being-resized {
  background-color: yellow;
}

/* when hovering resizable th */
th:hover .resize-handle {
  opacity: 0.3;
}
```
