<template>
  <div>
    <h1>Todo List</h1>
    <input type="text" /><button>Add</button>
    <ul>
      <li><span>abc</span> <button>Remove</button></li>
      <li><span class="completed">def</span> <button>Remove</button></li>
      <li><span>ghi</span> <button>Remove</button></li>
    </ul>
  </div>
</template>

<script setup lang="ts"></script>

<style scoped>
.completed {
  text-decoration: line-through;
  color: gray;
}
button {
  margin-left: 10px;
}
</style>
