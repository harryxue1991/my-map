<template>
  <div id="app">
    <Map></Map>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Map from './components/map.vue';

export default {
  name: 'app',
  components: {
    HelloWorld,Map
  }
}
</script>

<style>
*{
  margin:0;
  padding:0;
}
html,body{
  width:100%;
  height:100%;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  /* margin-top: 60px; */
  width:100%;
  height:100%;
}
</style>
