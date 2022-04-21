<template>
  <section>
    <!-- Table -->
    <v-table>
      <!-- Headers -->
      <thead>
        <tr>
          <th
              v-for="heading in $attrs.headers"
              :class="'text-' + $attrs.align"
              :value="heading.name"
              class="text-body-2 font-weight-bold clickable"
              @click="sortBy(heading)"
          >
            {{ heading.display }}
            <v-icon v-if="heading.sortable && heading.name !== currSorting" class="ml-4">
              mdi-filter-menu-outline
            </v-icon>
            <v-icon v-else-if="heading.name == currSorting && heading.order === 'ascending'" class="ml-4">
              mdi-sort-ascending
            </v-icon>
            <v-icon v-else-if="heading.name == currSorting && heading.order === 'descending'" class="ml-4">
              mdi-sort-descending
            </v-icon>
          </th>
        </tr>
      </thead>

      <!-- Items -->
      <tbody>
        <template v-for="(item, i) in displayedItems">
          <tr
              v-if="entryMatches($attrs.search, item)"
              :key="i"
              :class="(i % 2 === 0 && $attrs.striped !== undefined) ? 'bg-grey-lighten-4' : ''"
              class="clickable"
          >
            <td
                v-for="header in $attrs.headers"
                :class="'text-' + $attrs.align"
                @click="$attrs.rowClicked(item)"
            >
              {{ item[header.name] }}
            </td>
          </tr>
        </template>
      </tbody>
    </v-table>
    <!-- Pagination -->
    <div class="text-right">
      <v-pagination
          v-model="currPage"
          :length="pages"
      ></v-pagination>
    </div>
  </section>
</template>

<style scoped>
  .clickable {
    cursor: pointer
  }
  .selected {
    background-color: rgba(150,150,150,0.1)
  }
  .selected:hover {
    background-color: rgba(255,255,255,0.1)
  }
</style>

<script setup lang="ts">
import {computed, ref, useAttrs} from 'vue'

const attrs = <any>useAttrs()
let currSorting = ref("")
let currPage = ref(1)
let itemsPerPage = ref(10)

if(attrs.itemsPerPage !== undefined)
  itemsPerPage.value = parseInt(attrs.itemsPerPage)

// ---- Computed properties
let pages = computed(() => {
  return itemsPerPage.value === 0 ? 0 : Math.ceil(attrs.items.length / itemsPerPage.value)
})

let startIndex = computed(() => {
  return (currPage.value-1) * itemsPerPage.value
})

let endIndex = computed(() => {
  return startIndex.value + itemsPerPage.value
})

let displayedItems = computed(() => {
  return attrs.items.slice(startIndex.value, endIndex.value)
})

// ---- Functions
function sortBy(heading: any){
  if(heading.sortable){

    currSorting.value = heading.name
    heading.order = heading.order === 'ascending' ? 'descending' : 'ascending'

    // Sort descending
    if(heading.order === 'descending')
      displayedItems
          .value
          .sort((a:any, b:any) => (a[heading.name] < b[heading.name]) ? 1 : -1)

    // Sort ascending
    else
      displayedItems
          .value
          .sort((a:any, b:any) => (a[heading.name] > b[heading.name]) ? 1 : -1)
  }
}
function entryMatches(search: any, item: any){
  if(search !== undefined && search.length > 0){
    // If any obj entry matches the query
    let matching = false;
    Object.values(item).forEach((value: any) => {
        if(value.toString().toLowerCase().includes(search.toLowerCase()) && !matching)
          matching = true
    })
    return matching

  // Default
  } else
    return true
}

</script>