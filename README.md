# Tiny Vue 3 Datatable
👉 Built with [GitHub CoPilot!](https://copilot.github.com)

A one-file datatable for Vue 3 based on Vuetify 3.

### Usage
Check the [example file](example.vue) for the correct implementation!
```
<data-table
    :headers="tableHeaders"
    :items="tableItems"
    align="left"
    :search="searchEntry"
    itemsPerPage="10"
    :rowClicked="handleSelectedRow"
    striped
/>
```
### Features
- [x] Searchable items
- [x] Sortable items by column (multiple)
- [x] Pagination
- [x] Row click selection
- [x] For each item, values are displayed wrt the order of the headings (using heading.name as key)
- [ ] Custom entries with slots


