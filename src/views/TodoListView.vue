<template>
  <div>
    <h1>Todo List</h1>
    <input type="text" /><button>Add</button>
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <span>{{ index + 1 }}.</span><span>{{ todo.text }}</span>
        <button @click="removeTodo(index)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
interface Todo {
  text: string
  completed: boolean
}
function removeTodo(index: number) {
  todos.value.splice(index, 1)
}
const todos = ref<Todo[]>([
  {
    text: 'abc',
    completed: false,
  },
  {
    text: 'def',
    completed: true,
  },
  {
    text: 'ghi',
    completed: false,
  },
])
</script>

<style scoped>
.completed {
  text-decoration: line-through;
  color: gray;
}
button {
  margin-left: 10px;
}
</style>
