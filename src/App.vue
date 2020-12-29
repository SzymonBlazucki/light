<template>
  <div>
  <svgDraw msg="Simple light beam visualization"/>
  </div>
</template>

<script>
import svgDraw from './components/svgDraw.vue';

export default {
  name: 'App',
  components: {
    svgDraw,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
