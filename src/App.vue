<template>
  <div id="app">
      <headerComponent></headerComponent>
      <gameComponent></gameComponent>
  </div>
</template>

<script>
import headerComponent from "@/components/header.vue";
import gameComponent from "@/components/game";

export default {
  name: 'App',
  components: {
headerComponent,gameComponent,
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
  margin-top: 60px;
    width: 100%;
}

    input
    {
        width:14rem;
        padding:.6rem .6rem;
    }
</style>
