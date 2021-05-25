<template>
  <div id="app">
    <div class="cell cell-map">
      <map-container :geoJson="geoJson" v-on:select="selected = $event"></map-container>
    </div>
    <div class="cell cell-edit">
      <edit :geoJson="geoJson" v-on:change="geoJson = $event"></edit>
    </div>
    <div class="cell cell-inspect">
      <inspect :feature="selected"></inspect>
    </div>
  </div>
</template>

<script>
import Edit from './components/Edit.vue'
import Inspect from './components/Inspect.vue'
import MapContainer from './components/MapContainer.vue'
import suburbs from './suburbs.json'

export default {
  components: { MapContainer, Edit, Inspect },
  name: 'App',
  data: () => ({
    geoJson: suburbs,
    selected: undefined
  })
}
</script>

<style>
html, body {
  height: 100%;
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  height: 100%;
  display: grid;
  grid-template-columns: 100vh;
  grid-auto-rows: 1fr;
  gap: 1rem;
  padding: 1rem;
  box-sizing: border-box;
}

.cell {
  border-radius: 4px;
  background-color: lightgrey;
}

.cell-map {
  grid-column: 1;
  grid-row-start: 1;
  grid-row-end: 3;
}

.cell-edit {
  grid-column: 2;
  grid-row: 1;
}

.cell-inspect {
  grid-column: 2;
  grid-row: 2;
}
</style>
