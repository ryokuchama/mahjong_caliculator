<template>
  <div id="app">
    <marjong msg="🀄️Marjong Caliculator🀄️"/>
  </div>
</template>

<script>
import marjong from "./components/marjong";

export default {
  name: "App",
  components: {
    marjong
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
