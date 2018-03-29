<template>
  <div id="app">
    <Avery/>
  </div>
</template>

<script>
import Avery from './components/Avery.vue'

export default {
  name: 'app',
  components: {
    Avery
  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}
</style>
