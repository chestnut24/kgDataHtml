<template>
  <div id="app">
    <kgSystem></kgSystem>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import kgSystem from "@/components/kgSystem";

export default {
  name: 'App',
  components: {
    // HelloWorld
    kgSystem
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /*margin-top: 60px;*/
}
</style>
