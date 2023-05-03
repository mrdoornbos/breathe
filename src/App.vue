<template>
  <div id="app">
    <BreathingVisual />
  </div>
</template>

<script>
import BreathingVisual from './components/BreathingVisual.vue';

export default {
  components: {
    BreathingVisual,
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
