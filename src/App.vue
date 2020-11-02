<template>
  <div id="app">
    <G6Editor
      mode="edit"
      :data="data"
    >

    </G6Editor>
  </div>
</template>

<script>
import G6Editor from './components/G6Editor'
import JSONData from './data'

export default {
  name: 'app',
  components:{G6Editor},
  data: () => ({
    data: JSONData
  })
}
</script>

<style>
html,
body {
  overflow: hidden;
  margin: 0;
  font-size: 12px;
}
#app {
  margin: 0;
  padding: 0;
}
</style>
