<template>
  <div>
    <input
      v-model="newTodo"
      class="new-todo"
      autofocus
      autocomplete="off"
      placeholder="What needs to be done?"
      @keyup.enter="addTodo"
    />
    <section v-cloak v-show="todos.length" class="main">
      <ul class="todo-list">
        <li
          v-for="todo in todos"
          :key="todo.id"
          class="todo"
          @click="removeTodo(todo)"
        >
          <p>{{ todo.title }}</p>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
const STORAGE_KEY = 'TODO'
export default {
  name: 'TodoList',
  data() {
    return {
      todos: this.fetchTodos(),
      nextId: 0,
      newTodo: '',
    }
  },
  watch: {
    todos: {
      handler() {
        this.saveToStorage()
      },
      deep: true,
    },
  },
  methods: {
    fetchTodos() {
      const todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
      todos.forEach((todo, index) => {
        todo.id = index
      })
      this.nextId = todos.length
      return todos
    },
    removeTodo(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1)
    },
    addTodo() {
      const value = this.newTodo && this.newTodo.trim()
      if (!value) {
        return
      }
      this.todos.push({
        id: this.nextId++,
        title: value,
      })
      this.newTodo = ''
    },
    saveToStorage() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos))
    },
  },
}
</script>

<style scoped>
.main {
  position: relative;
  z-index: 2;
  border-top: 1px solid #e6e6e6;
}
.todo-list {
  margin: 0;
  padding: 0;
  list-style: none;
}
.todo-list li {
  position: relative;
  font-size: 24px;
  border-bottom: 1px solid #ededed;
}
.new-todo {
  position: relative;
  margin: 0;
  width: 100%;
  font-size: 24px;
  font-family: inherit;
  font-weight: inherit;
  line-height: 1.4em;
  border: 0;
  color: inherit;
  box-sizing: border-box;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding: 16px;
  border: none;
  background: rgba(0, 0, 0, 0.003);
  box-shadow: inset 0 -2px 1px rgba(0, 0, 0, 0.03);
}
</style>
