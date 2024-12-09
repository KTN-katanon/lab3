<template>
  <div id="page">
    <h1>User Management</h1>
    <div>
      <input type="text" placeholder="Enter email" v-model="form.email" />
      <input type="password" placeholder="Enter password" v-model="form.password" />
      <button @click="save">Save</button>
      <button>Cancel</button>
    </div>
    <div>
      <table border="1">
        <thead>
          <tr>
            <th>ID</th>
            <th>Email</th>
            <th>Password</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in users" :key="user.id">
            <td>{{ user.id }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.password }}</td>
            <td>
              <button @click="editUser(user.id)">Edit</button
              ><button @click="deleteUser(user.id)">Delete</button>
            </td>
          </tr>
          <tr v-if="users.length === 0">
            <td colspan="4">No Data</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'
interface User {
  id: number
  email: string
  password: string
}
const form = ref<User>({ id: 0, email: '', password: '' })
const users = ref<User[]>([])
const lastId = ref(1)
const editedIndex = ref<number | null>(null)
function save() {
  if (editedIndex.value != null) {
    updateUser()
  } else {
    addUser()
  }
}
function editUser(id: number) {
  const index = users.value.findIndex(function (item) {
    return item.id === id
  })
  editedIndex.value = index
  form.value = { ...users.value[index] }
}
function updateUser() {
  if (editedIndex.value !== null) {
    users.value[editedIndex.value!] = { ...form.value }
    clearForm()
    editedIndex.value = null
  }
}
function addUser() {
  users.value.push({ ...form.value, id: lastId.value })
  lastId.value++
  clearForm()
}
function deleteUser(id: number) {
  const index = users.value.findIndex(function (item) {
    return item.id === id
  })
  users.value.splice(index, 1)
}
function clearForm() {
  form.value = { id: 0, email: '', password: '' }
}
</script>

<style scoped>
#page {
  text-align: center;
}
table {
  margin: 20px auto;
  width: 80%;
  border-style: solid;
  border-collapse: collapse;
}
input,
button {
  margin-left: 4px;
}
td,
th {
  padding: 8px;
  text-align: center;
}
</style>
