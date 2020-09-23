<template>
  <div id="app" class="container">
    <todoComp />
  </div>
</template>

<script>
import todoComp from './components/todoComp.vue'

export default {
  name: 'App',
  components: {
    todoComp
  }
}
</script>

<style>

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body{
  margin-top: 100px;
  background-color: #bcf7ee;
}
.container{
  width: 500px;
  margin: 0 auto;
  font-size: 3rem;
}
@media only screen and (max-width: 500px) {
  .app{
    width: 100vw;
  }
  .container{
    width: 100%;
  }
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-size: 1.5rem;
  color: #2c3e50;
}
</style>
