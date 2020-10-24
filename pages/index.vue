<template>
  <div class="container">
    <div>
      <Logo />
      <TodoList />
    </div>
  </div>
</template>

<script>
import TodoList from '../components/TodoList'
export default {
  components: { TodoList },
}
</script>

<style>
.container {
  margin: 0 auto;
  max-width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
