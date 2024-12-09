<template>
  <div>
    <h1>Todo List</h1>
    <div>
      <input
        type="text"
        v-model="newTodo"
        placeholder="Add a new task"
        @keyup.enter="addTodo"
      /><button @click="addTodo">Add</button>
    </div>
    <div>
      <button @click="filter = 'completed'">Completed</button>
      <button @click="filter = 'not_completed'">Not Complete</button>
      <button @click="filter = 'all'">All</button>
    </div>
    <div v-if="filterTodos.length === 0">No items</div>
    <ul v-if="filterTodos.length > 0">
      <li v-for="todo in filterTodos" :key="todo.id">
        <span :class="{ completed: todo.completed }" @click="toggle(todo.id)"
          >{{ todo.id }}{{ todo.text }}</span
        >
        <button @click="removeTodo(todo.id)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
interface Todo {
  id: number
  text: string
  completed: boolean
}
let lastId: number = 3
function addTodo() {
  if (newTodo.value.trim() === '') return
  todos.value.push({
    id: lastId,
    text: newTodo.value.trim(),
    completed: false,
  })
  lastId++
  newTodo.value = ''
}
function removeTodo(id: number) {
  const index = todos.value.findIndex(function (item) {
    return item.id === id
  })
  todos.value.splice(index, 1)
}
function toggle(id: number) {
  const index = todos.value.findIndex(function (item) {
    return item.id === id
  })
  todos.value[index].completed = !todos.value[index].completed
}
const todos = ref<Todo[]>([
  {
    id: 0,
    text: 'abc',
    completed: false,
  },
  {
    id: 1,
    text: 'def',
    completed: true,
  },
  {
    id: 2,
    text: 'ghi',
    completed: false,
  },
])
const filterTodos = computed(function () {
  if (filter.value === 'completed') {
    return todos.value.filter(function (item) {
      return item.completed === true
    })
  }
  if (filter.value === 'not_completed') {
    return todos.value.filter(function (item) {
      return item.completed === false
    })
  } else {
    return todos.value
  }
})
const filter = ref<string>('all') // all, completed, not completed
const newTodo = ref<string>('')
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
