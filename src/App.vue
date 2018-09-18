<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <!-- <HelloWorld/> -->
    <!-- <dthree/> -->
    <tree/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld';
import dthree from './components/dthree';
import tree from './components/tree';

export default {
  name: 'App',
  components: {
    HelloWorld,
    dthree,
    tree,
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
