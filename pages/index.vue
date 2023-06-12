<template>
  <v-alert closable :type="alert.type" :model-value="alert.opened" class="mb-6">
    {{ alert.message }}
  </v-alert>

  <h1 class="text-center text-xl mb-4">Lista de Veículos</h1>
  <button @click="openCreateModal()" class="bg-blue-500 hover:bg-blue-700 text-white px-3 py-2 rounded-lg">Novo</button>

  <v-table>
    <thead>
      <tr>
        <td>ID</td>
        <td>Marca</td>
        <td>Modelo</td>
        <td>Ano</td>
        <td>Cor</td>
        <td></td>
      </tr>
    </thead>
    <tbody>
      <tr v-for="car in cars" :key="car.id" class="hover:bg-sky-100">
        <td>{{ car.id }}</td>
        <td>{{ car.brand }}</td>
        <td>{{ car.model }}</td>
        <td>{{ car.year }}</td>
        <td>{{ car.color }}</td>
        <td class="">
          <button @click="openEditModal(car.id)" class="bg-sky-500 text-white hover:bg-sky-700 px-3 py-2 rounded-lg mr-3">Editar</button>
          <button @click="openDeleteModal(car.id)" class="bg-sky-500 text-white hover:bg-sky-700 px-3 py-2 rounded-lg">Excluir</button>
        </td>
      </tr>
    </tbody>
  </v-table>

  <v-dialog v-model="createDialog" width="auto">
    <v-card>
      <v-card-title>
        <h4 class="text-center text-lg mb-4">Adicionar novo veículo</h4>
      </v-card-title>
      <v-card-text>
        <template class="flex flex-col gap-2">
          <label for="brand">Marca</label>
          <input type="text" v-model="form.brand.value" id="brand" @keydown="form.brand.error = ''"
            :class="[form.brand.error ? 'is-invalid' : '']" />
          <span v-if="form.brand.error" class="text-red">{{ form.brand.error }}</span>
          <label for="model">Modelo</label>
          <input type="text" v-model="form.model.value" id="model" @keydown="form.model.error = ''"
            :class="[form.model.error ? 'is-invalid' : '']" />
          <span v-if="form.model.error" class="text-red">{{ form.model.error }}</span>
          <label for="color">Cor</label>
          <input type="text" v-model="form.color.value" id="color" @keydown="form.color.error = ''"
            :class="[form.color.error ? 'is-invalid' : '']" />
          <span v-if="form.color.error" class="text-red">{{ form.color.error }}</span>
          <label for="year">Ano de fabricação</label>
          <input type="text" v-model="form.year.value" id="year" @keydown="form.year.error = ''"
            :class="[form.year.error ? 'is-invalid' : '']" />
          <span v-if="form.year.error" class="text-red">{{ form.year.error }}</span>
        </template>
      </v-card-text>
      <v-card-actions class="flex justify-center mx-auto">
        <button @click="createCar" class="bg-green-500 hover:bg-green-700 text-white px-3 py-2 rounded-lg mr-4">Salvar</button>
        <button @click="createDialog = false" class="bg-sky-500 hover:bg-sky-700 text-white px-3 py-2 rounded-lg">Fechar</button>
      </v-card-actions>
    </v-card>
  </v-dialog>

  <v-dialog v-model="editDialog" width="auto">
    <v-card>
      <v-card-title>
        <h4 class="text-center text-lg mb-4">Editar informações do veículo</h4>
      </v-card-title>
      <v-card-text>
        <template class="flex flex-col gap-2">
          <label for="brand">Marca</label>
          <input type="text" v-model="form.brand.value" id="brand" @keydown="form.brand.error = ''"
            :class="[form.brand.error ? 'is-invalid' : '']" />
          <span v-if="form.brand.error" class="text-red">{{ form.brand.error }}</span>
          <label for="model">Modelo</label>
          <input type="text" v-model="form.model.value" id="model" @keydown="form.model.error = ''"
            :class="[form.model.error ? 'is-invalid' : '']" />
          <span v-if="form.model.error" class="text-red">{{ form.model.error }}</span>
          <label for="color">Cor</label>
          <input type="text" v-model="form.color.value" id="color" @keydown="form.color.error = ''"
            :class="[form.color.error ? 'is-invalid' : '']" />
          <span v-if="form.color.error" class="text-red">{{ form.color.error }}</span>
          <label for="year">Ano de fabricação</label>
          <input type="text" v-model="form.year.value" id="year" @keydown="form.year.error = ''"
            :class="[form.year.error ? 'is-invalid' : '']" />
          <span v-if="form.year.error" class="text-red">{{ form.year.error }}</span>
        </template>
      </v-card-text>
      <v-card-actions class="flex justify-center mx-auto">
        <button @click="editCar" class="bg-green-500 hover:bg-green-700 text-white px-3 py-2 rounded-lg mr-4">Salvar</button>
        <button @click="editDialog = false" class="bg-sky-500 hover:bg-sky-700 text-white px-3 py-2 rounded-lg">Fechar</button>
      </v-card-actions>
    </v-card>
  </v-dialog>

  <v-dialog v-model="deleteDialog" width="auto">
    <v-card>
      <v-card-title>
        <h4 class="text-center text-lg mb-4">Excluir registro</h4>
      </v-card-title>
      <v-card-text class="text-center">
        <p>Deseja excluir o registro do veículo <span class="font-bold">{{ currentCar.id }}: {{ currentCar.brand }} {{
          currentCar.model }}</span>?</p>
      </v-card-text>
      <v-card-actions class="flex justify-center mx-auto">
        <button @click="deleteCar" class="bg-red-500 hover:bg-red-700 text-white px-3 py-2 rounded-lg mr-4">Sim</button>
        <button @click="deleteDialog = false" class="bg-sky-500 hover:bg-sky-700 text-white px-3 py-2 rounded-lg">Não</button>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script setup>
useHead({ title: "Catálogo de veículos" })

const { data } = await useFetch('http://localhost:8000/api/cars')
const cars = ref(data.value.data[0])
const form = reactive({
  id: 0,
  brand: { value: '', error: '' },
  model: { value: '', error: '' },
  year: { value: 0, error: '' },
  color: { value: '', error: '' }
});
const currentCar = ref(0)
const createDialog = ref(false)
const editDialog = ref(false)
const deleteDialog = ref(false)
const alert = reactive({ type: 'info', opened: false, message: '' })

function openCreateModal() {
  resetForm()
  createDialog.value = true;
}

async function createCar(event) {
  const formData = new FormData()
  formData.set('brand', form.brand.value)
  formData.set('model', form.model.value)
  formData.set('color', form.color.value)
  formData.set('year', form.year.value)
  const { data, error } = await useFetch(`http://localhost:8000/api/cars/`, { method: 'POST', body: formData })
  if (!data.value) {
    if (error) {
      const errors = error.value.data.errors
      if (errors.brand) form.brand.error = errors.brand[0]
      if (errors.model) form.model.error = errors.model[0]
      if (errors.year) form.year.error = errors.year[0]
      if (errors.color) form.color.error = errors.color[0]
    }
  } else {
    createDialog.value = false
    refreshTableData()
    alert.opened = true
    alert.message = data.value.message
    alert.type = 'success'
  }
}

function openEditModal(id) {
  resetForm()
  currentCar.value = cars.value.filter((car) => { return car.id == id })[0];
  form.id = currentCar.value.id
  form.brand.value = currentCar.value.brand
  form.model.value = currentCar.value.model
  form.color.value = currentCar.value.color
  form.year.value = currentCar.value.year
  editDialog.value = true;
}

async function editCar(event) {
  const formData = new FormData()
  formData.set('_method', 'PUT')
  formData.set('brand', form.brand.value)
  formData.set('model', form.model.value)
  formData.set('color', form.color.value)
  formData.set('year', form.year.value)
  const { data, error } = await useFetch(`http://localhost:8000/api/cars/${form.id}`, { method: 'POST', body: formData })
  if (!data.value) {
    if (error) {
      const errors = error.value.data.errors
      if (errors.brand) form.brand.error = errors.brand[0]
      if (errors.model) form.model.error = errors.model[0]
      if (errors.year) form.year.error = errors.year[0]
      if (errors.color) form.color.error = errors.color[0]
    }
  } else {
    editDialog.value = false
    refreshTableData()
    alert.opened = true
    alert.message = data.value.message
    alert.type = 'success'
    resetForm()
  }
}

function openDeleteModal(id) {
  resetForm()
  currentCar.value = cars.value.filter((car) => { return car.id == id })[0];
  deleteDialog.value = true;
}

async function deleteCar(event) {
  const formData = new FormData()
  formData.set('_method', 'DELETE')
  const { data, error } = await useFetch(`http://localhost:8000/api/cars/${currentCar.value.id}`, { method: 'POST', body: formData })
  if (!data.value) {
    console.log(error.value)
  } else {
    deleteDialog.value = false
    refreshTableData()
    alert.opened = true
    alert.message = data.value.message
    alert.type = 'success'
    resetForm()
  }
}

async function refreshTableData() {
  const { data } = await useFetch('http://localhost:8000/api/cars')
  cars.value = data.value.data[0]
}

function resetForm() {
  form.id = 0,
  form.brand.value = ''
  form.brand.error = ''
  form.model.value = ''
  form.model.error = ''
  form.color.value = ''
  form.color.error = ''
  form.year.value = ''
  form.year.error = ''
}

watch(alert, () => {
  if (alert.opened)
  setTimeout(() => {
    alert.opened = false
  }, 3000)
})
</script>

<style scoped>
input {
  border: 1px solid black;
  border-radius: 5px;
  padding: .5rem 1rem;
}

input.is-invalid {
  border-color: red;
}
</style>