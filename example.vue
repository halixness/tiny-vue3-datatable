<template>
    <div>
      <section class="d-flex flex-row nowrap justify-space-between">
          <h2 class="text-h2 mb-2">
              Tiny Vue 3 DataTable
          </h2>
          <v-text-field
              v-model="searchEntry"
              append-inner-icon="mdi-magnify"
              label="Search..."
          />
      </section>
      <data-table
          :headers="tableHeaders"
          :items="tableItems"
          align="left"
          :search="searchEntry"
          itemsPerPage="10"
          :rowClicked="handleSelectedRow"
          striped
      />
    </div>  
</template>

<style scoped>
</style>

<script setup lang="ts">

import {computed, ref} from "vue";
import DataTable from "TinyDataTable.vue";

let searchEntry = ref("")

// ----- Computed
let tableHeaders = ref([
    {
      display: 'Name',
      name: 'name',
      sortable: true,
    },
    {
      display: 'Calories',
      name: 'calories',
      sortable: true,
    },
    {
      display: 'Fat',
      name: 'fat',
      sortable: false,
    },
    {
      display: 'Carbs',
      name: 'carbs',
      sortable: false,
    },
    {
      display: 'Protein',
      name: 'protein',
      sortable: false,
    },
])
let tableItems = ref([
    {
      name: 'Eclair',
      calories: '262',
      fat: '16',
      carbs: '24',
      protein: '6',
    },
    {
      name: 'Cupcake',
      calories: '305',
      fat: '4',
      carbs: '67',
      protein: '4',
    },
    {
      name: 'Gingerbread',
      calories: '356',
      fat: '16',
      carbs: '49',
      protein: '16',
    },
])

// ----- Functions
function handleSelectedRow(item: any){
  console.log(item)
  // your action here!
}
</script>


