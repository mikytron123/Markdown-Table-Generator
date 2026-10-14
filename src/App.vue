<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import { ref } from 'vue'
import type { Ref } from 'vue'
const rownum = ref(1);
const colnum = ref(1);
const table:Ref<HTMLTableElement | null> = ref(null);
const output = ref("") 


function tomarkdown(){
  const data = Array.from(table.value?.rows ?? [])
    .filter(row => row.hasChildNodes())
    .map(row => Array.from(row.cells).map(cell => cell.innerText))
  const firstRow = data[0]
  if (!firstRow) {
    output.value = ""
    return
  }

  let result = "| ".repeat(firstRow.length) + "|\n"
  result += "|:----:".repeat(firstRow.length) + "|\n"
  for (const row of data) {
    result += "| " + row.join(" | ") + " |\n"
  }
  output.value = result;

}
</script>

<template>
  <h1> Convert Html Table to Markdown</h1>
  <div>
  <p><label for="rows">Rows</label></p>

  <input type="number" id="rownum" name="rows" min="1" max="20" v-model="rownum">

  <p><label for="cols">Columns</label></p>

  <input type="number" id="colnum" name="cols" min="1" max="20" v-model="colnum">
  <p></p>
</div>

  <div>
    <table ref="table" id="table" border="3">
      <thead>
        <tr v-for="n in colnum"></tr>
      </thead>
      <tbody>
        <tr v-for="x in rownum">
          <td v-for="y in colnum" contenteditable='true'></td>
        </tr>
      </tbody>
    </table>
  </div>
  <p></p>
  <div>
    <button name="convert" type="button" @click="tomarkdown">
      Convert To Markdown
    </button>
  </div>
  <p><label for="output">Output</label></p>
  <textarea title="output" v-model="output" id="result" rows="18" spellcheck="false" disabled></textarea>
</template>

<style scoped>
td{
  height: 20px;
  width: 100px;
}

</style>
