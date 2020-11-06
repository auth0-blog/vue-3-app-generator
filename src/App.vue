<template>
  <Generator />
</template>

<script>
import Generator from './components/Generator.vue'

export default {
  name: 'App',
  components: {
    Generator
  }
}
</script>

<style lang="scss">
html, body {
  margin: 0;
  height: 100%;
  background: #f9f9f9;
}
.container {
  padding-top: 20px;;
}
</style>
