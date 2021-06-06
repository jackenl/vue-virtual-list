<template>
  <div id="app">
    <VirtualList :listData="list" :itemSize="100"></VirtualList>
  </div>
</template>

<script>
import VirtualList from './components/VirtualList'

export default {
  name: 'App',
  components: {
    VirtualList
  },
  data() {
    return {
      list: []
    }
  },
  created() {
    let index = 0
    while (index < 100000) {
      this.list.push({
        id: index,
        value: index
      })
      index++
    }
  }
}
</script>

<style>
html {
  height: 100%;
}
body {
  height: 100%;
}
#app {
  height: 100%;
}
</style>
