<template>
  <div id="app" class="container">
    <img alt="Vue logo" class="logo" src="./assets/logo.png">
    <todo-list/>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue'



export default {
  name: 'app',
  components: {
    TodoList
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
  font-size: 24px;
  color: #2c3e50;
  padding: 10px;
}


*{
  box-sizing: border-box;
}

.container{
  max-width: 600px;
  margin: 0 auto;
}

.logo{
  display: block;
  margin: 20px auto;
  height: 75px;
}
</style>
