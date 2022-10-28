<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import { ref } from 'vue'
import type { Ref } from 'vue'
const rownum = ref(1);
const colnum = ref(1);
const table:Ref<HTMLTableElement | null> = ref(null);
const output = ref("") 


function tomarkdown(event:Event){
  let data = Array.from(table.value?.rows as HTMLCollection).filter(x=>x.hasChildNodes()).map(y=>Array.from(y.children).map(z=>(z as HTMLTableCellElement).innerText))
  let result = ""
  for (let i = 0; i < data.length+2; i++) {
    if (i==0) {
      result = result + "| ".repeat(data[0].length) + "|\n";
      continue;
    }
    if (i==1){
      result = result + "|:----:".repeat(data[0].length) + "|\n";
      continue;
    }
    result += "| " +  data[(i-2)].join(" | ") + " |\n"
    
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
        <tr v-for="x in colnum">
          <td v-for="y in rownum" contenteditable='true' style="height:20px;width:100px"></td>
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

</style>
