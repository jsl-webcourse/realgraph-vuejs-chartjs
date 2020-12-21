<template>
  <div id="app">
    <Header />
    <MyChart class="chartjs" />
  </div>
</template>

<script>
import MyChart from './components/MyChart.vue';
import Header from './components/Header.vue';

export default {
  name: 'App',
  components: {
    MyChart,
    Header
  }
}
</script>

<style>
body {
  margin: 0px;
  padding: 0px;
}
</style>