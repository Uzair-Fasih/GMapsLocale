<template>
  <div id="app">
    <Starter />
    <Map />
  </div>
</template>

<script>
import Map from './components/Map'
import Starter from './components/Starter'

export default {
  name: 'App',
  components: {
    Starter,
    Map
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  font-size: 10px;
}

.grid {
  display: grid;
  grid-template-columns: [gutter-start] 1fr [content-start] 75em [content-end] 1fr [gutter-end];
  grid-template-rows: [gutter-start] 1fr [gutter-end];
}

.grid-content_wide {
  grid-row-start: guttter-start;
  grid-row-end: guttter-end;
  grid-column: guttter-start;
  grid-column-end: guttter-end;
}

.grid-content_narrow {
  grid-row-start: guttter-start;
  grid-row-end: guttter-end;
  grid-column: content-start;
  grid-column-end: content-end;
}
</style>

<style scoped>
#app {
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  background-color: black;
  min-height: 100vh;
}
</style>
