<template>
  <div id="page">
    <h1>Product Management</h1>
    <div>
      <input type="text" placeholder="Enter name" v-model="form.name" />
      <input type="number" placeholder="Enter Price" value="0" v-model="form.price" />
      <input type="checkbox" checked v-model="form.status" />
      <button @click="save">Save</button>
      <button @click="cancel">Cancel</button>
    </div>
    <div>
      <button @click="setFilter('all')">All</button>
      <button @click="setFilter('On_Shelf')">On Shelf</button>
      <button @click="setFilter('Off_Shelf')">Off Shelf</button>
    </div>
    <div>
      <table border="1">
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Price</th>
            <th>Status</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="product in filteredProducts" :key="product.id">
            <td>{{ product.id }}</td>
            <td>{{ product.name }}</td>
            <td>{{ product.price }}</td>
            <td>{{ product.status ? 'On Shelf' : 'Off Shelf' }}</td>
            <td>
              <button @click="editProduct(product.id)">Edit</button
              ><button @click="deleteProduct(product.id)">Delete</button>
            </td>
          </tr>
          <tr v-if="filteredProducts.length === 0">
            <td colspan="5">No Data</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, computed } from 'vue'
interface Product {
  id: number
  name: string
  price: number
  status: boolean
}
const form = ref<Product>({ id: 0, name: '', price: 0, status: true })
const products = ref<Product[]>([])
const lastId = ref(1)
const editedIndex = ref<number | null>(null)
const filter = ref<string>('all') // 'all', 'On_Shelf', 'Off_Shelf'
function save() {
  if (editedIndex.value != null) {
    updateProduct()
  } else {
    addProduct()
  }
}
function editProduct(id: number) {
  const index = products.value.findIndex(function (item) {
    return item.id === id
  })
  editedIndex.value = index
  form.value = { ...products.value[index] }
}
function updateProduct() {
  if (editedIndex.value !== null) {
    products.value[editedIndex.value!] = { ...form.value }
    clearForm()
    editedIndex.value = null
  }
}
function addProduct() {
  products.value.push({ ...form.value, id: lastId.value })
  lastId.value++
  clearForm()
}
function deleteProduct(id: number) {
  const index = products.value.findIndex(function (item) {
    return item.id === id
  })
  products.value.splice(index, 1)
}
function clearForm() {
  form.value = { id: 0, name: '', price: 0, status: true }
}
function cancel() {
  clearForm()
  editedIndex.value = null
}
const filteredProducts = computed(() => {
  if (filter.value === 'On_Shelf') {
    return products.value.filter((product) => product.status)
  } else if (filter.value === 'Off_Shelf') {
    return products.value.filter((product) => !product.status)
  } else {
    return products.value
  }
})
function setFilter(value: string) {
  filter.value = value
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
