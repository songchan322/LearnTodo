<template>
  <div id="app">
    <todo-list></todo-list>
  </div>
</template>

<script>
  import TodoList from './components/TodoList.vue'
  export default {
    name: 'App',
    components: {
      TodoList
    }
  }
</script>

<style>
  html,
  body {
    margin: 0;
    padding: 0;
    background-color: #eee;
  }

  body {
    max-width: 550px;
    margin: 0 auto;
  }

  #app {
    border: 0;
    border-radius: 10px;
    background-color: #fff;
    margin: 150px auto;
    box-sizing: border-box;
  }
</style>
