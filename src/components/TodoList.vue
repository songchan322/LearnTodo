<template>
  <div id="todo-list">
    <header class="header">
      <i class="new-todo-icon"></i>
      <input
        @keyup.enter.prevent="saveTodo"
        v-model.trim="newTodo"
        class="input-box"
        type="text"
        placeholder="Enter todos here..."
      />
      <i @click="saveTodo" class="add-icon" v-show="newTodo.length > 0"></i>
    </header>
    <section class="main">
      <div class="completed-wrapper">
        <input type="checkbox" id="toggle-all" class="toggle-all" v-model="allDone" />
        <label for="toggle-all">全部完成</label>
        <button @click="deleteCompleted" class="clear-completed">清除已完成</button>
      </div>
      <div class="list-wrapper">
        <div v-for="(todo, index) in todos" :key="index" class="todo-list-item">
          <input type="checkbox" :id="todo.id" v-model="todo.completed" />
          <label :for="todo.id" :class="{ 'is-completed': todo.completed }" class="todo-item-text">{{
            todo.title
          }}</label>
          <button @click="deleteTodo(index)" class="deleteTodo">X</button>
        </div>
      </div>
      <footer class="footer">
        <span v-show="remaining">剩余{{ remaining }}个任务</span>
        <span v-show="!remaining">全部完成！放松一下吧！</span>
      </footer>
    </section>
  </div>
</template>

<script>
  function guid() {
    function S4() {
      return (((1 + Math.random()) * 0x10000) | 0).toString(16).substring(1)
    }
    return S4() + S4() + '-' + S4() + '-' + S4() + '-' + S4() + '-' + S4() + S4() + S4()
  }

  var filters = {
    all: function(todos) {
      return todos
    },
    active: function(todos) {
      return todos.filter(function(todo) {
        return !todo.completed
      })
    },
    completed: function(todos) {
      return todos.filter(function(todo) {
        return todo.completed
      })
    }
  }

  export default {
    name: 'TodoList',
    data() {
      return {
        newTodo: '',
        todos: [
          { id: 1, title: 'Example todos', completed: false },
          { id: 2, title: 'Hello World', completed: true }
        ]
      }
    },
    methods: {
      saveTodo() {
        if (this.newTodo != '') {
          this.todos.push({
            id: guid(),
            title: this.newTodo,
            completed: false
          })
          this.newTodo = ''
        }
      },
      deleteTodo(index) {
        this.todos.splice(index, 1)
      },
      deleteCompleted() {
        this.todos = filters.active(this.todos)
      }
    },
    computed: {
      remaining: function() {
        return filters.active(this.todos).length
      },
      allDone: {
        get: function() {
          return this.remaining === 0
        },
        set: function(value) {
          this.todos.forEach(function(todo) {
            todo.completed = value
          })
        }
      }
    }
  }
</script>

<style lang="scss">
  #todo-list {
    padding: 15px;
    position: relative;
    font-size: 14px;
    font-weight: 200;
  }

  @mixin myicon {
    display: block;
    color: #444;
    position: absolute;
    height: 65px;
    width: 65px;
    z-index: 1;
    background-repeat: no-repeat;
    background-position: center;
    opacity: 0.8;
  }
  .new-todo-icon {
    @include myicon();
    top: 15px;
    left: 15px;
    background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij48cGF0aCBkPSJNNiAxMmgxMHYxaC0xMHYtMXptNy44MTYtM2gtNy44MTZ2MWg5LjA0N2MtLjQ1LS4yODMtLjg2My0uNjE4LTEuMjMxLTF6bS03LjgxNi0yaDYuNWMtLjEzNC0uMzItLjIzNy0uNjU2LS4zMTktMWgtNi4xODF2MXptMTMgMy45NzV2Mi41NjhjMCA0LjEwNy02IDIuNDU3LTYgMi40NTdzMS41MTggNi0yLjYzOCA2aC03LjM2MnYtMjBoOS41Yy4zMTItLjc0OS43NjMtMS40MjQgMS4zMTYtMmgtMTIuODE2djI0aDEwLjE4OWMzLjE2MyAwIDkuODExLTcuMjIzIDkuODExLTkuNjE0di0zLjg4NmMtLjYyMy4yNi0xLjI5Ny40MjEtMiAuNDc1em00LTYuNDc1YzAgMi40ODUtMi4wMTUgNC41LTQuNSA0LjVzLTQuNS0yLjAxNS00LjUtNC41IDIuMDE1LTQuNSA0LjUtNC41IDQuNSAyLjAxNSA0LjUgNC41em0tMi4xNTYtLjg4MmwtLjY5Ni0uNjk2LTIuMTE2IDIuMTY5LS45OTItLjk0MS0uNjk2LjY5NyAxLjY4OCAxLjYzNyAyLjgxMi0yLjg2NnoiLz48L3N2Zz4=');
  }
  .add-icon {
    @include myicon();
    cursor: pointer;
    top: 15px;
    right: 15px;
    background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij48cGF0aCBkPSJNMTIgMmM1LjUxNCAwIDEwIDQuNDg2IDEwIDEwcy00LjQ4NiAxMC0xMCAxMC0xMC00LjQ4Ni0xMC0xMCA0LjQ4Ni0xMCAxMC0xMHptMC0yYy02LjYyNyAwLTEyIDUuMzczLTEyIDEyczUuMzczIDEyIDEyIDEyIDEyLTUuMzczIDEyLTEyLTUuMzczLTEyLTEyLTEyem02IDEzaC01djVoLTJ2LTVoLTV2LTJoNXYtNWgydjVoNXYyeiIvPjwvc3ZnPg==');
  }

  .input-box {
    position: relative;
    box-sizing: border-box;
    width: 100%;
    padding: 16px 60px 16px 60px;
    border-radius: 10px;
    border: none;
    font-size: 28px;
    font-weight: 200;
    background: #efefef;
    color: #444;
    transition: background 0.3s ease;
    &:focus {
      background: #ddd;
      border: none;
      outline: none;
    }
  }

  .completed-wrapper {
    position: relative;
    font-size: inherit;
    font-weight: inherit;
    padding: 10px 0;
    border-bottom: 1px solid #ddd;
    display: flex;
  }

  .toggle-all {
    display: none;
  }

  .toggle-all + label {
    cursor: pointer;
    transition: color 0.3s ease;
    &:hover {
      color: #000;
    }
  }

  .clear-completed {
    font: inherit;
    font-size: inherit;
    font-weight: inherit;
    cursor: pointer;
    border-style: none;
    outline: none;
    padding: 0 15px;
  }

  .list-wrapper {
    padding: 5px 0;
  }

  .todo-list-item {
    padding: 5px 0;
    font-size: 18px;
    font-weight: 300;
  }

  .todo-item-text {
    display: inline-block;
  }

  .deleteTodo {
    float: right;
    height: 25px;
    width: 25px;
    font-size: inherit;
    font-weight: 100;
    border-style: none;
    cursor: pointer;
    transition: color 0.3s ease;
    &:hover {
      color: red;
    }
  }

  .is-completed {
    text-decoration: line-through;
  }

  .footer {
    border-top: 1px solid #ddd;
    padding-top: 10px;
  }
</style>
