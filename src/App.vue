<script setup>
import { ref, computed } from 'vue'
import CarCard from './components/CarCard.vue'
import CarFormDialog from './components/CarFormDialog.vue'

const cars = ref([
  { id: 1, name: 'Toyota', brand: 'Chaser', year: 1999, price: 1600000, image: 'https://i.pinimg.com/736x/fd/2a/32/fd2a320545d2650ea8d123031bde462c.jpg' },
  { id: 2, name: 'BMW', brand: 'M5 F90', year: 2021, price: 13000000, image: 'https://i.pinimg.com/736x/38/b3/15/38b3156f0386ad05c5e2d4539a7f1b9c.jpg' },
  { id: 3, name: 'Audi', brand: 'R8', year: 2023, price: 16000000, image: 'https://i.pinimg.com/736x/75/ae/b1/75aeb121dfeb675fbe8e2fe200079800.jpg' },
  { id: 4, name: 'Mercedes', brand: 'CLS63 AMG', year: 2014, price: 5900000, image: 'https://i.pinimg.com/736x/4e/e9/a0/4ee9a039050e95e765e41fd5e8b987de.jpg' }
])

const search = ref('')
const filterBrand = ref('')
const dialog = ref(false)
const editingCar = ref(null)
const name = ['Toyota', 'BMW', 'Mercedes', 'Audi', 'Lada', 'Kia', 'Hyundai']

const filteredCars = computed(() => {
  return cars.value.filter(car => {
    const matchSearch = car.brand.toLowerCase().includes(search.value.toLowerCase())
    const matchBrand = !filterBrand.value || car.name === filterBrand.value
    return matchSearch && matchBrand
  })
})

const openDialog = (car = null) => {
  editingCar.value = car ? { ...car } : { id: Date.now(), name: '', brand: '', year: 2020, price: 0, image: '' }
  dialog.value = true
}

const saveCar = (car) => {
  if (cars.value.find(c => c.id === car.id)) {
    const idx = cars.value.findIndex(c => c.id === car.id)
    cars.value[idx] = car
  } else {
    cars.value.push(car)
  }
  dialog.value = false
}

const deleteCar = (id) => {
  cars.value = cars.value.filter(c => c.id !== id)
}
</script>

<template>
  <v-app>
    <v-app-bar color="primary" density="compact">
      <v-app-bar-title>АвтоКаталог</v-app-bar-title>
      <v-spacer></v-spacer>
      <v-theme-toggle class="mr-2"></v-theme-toggle>
      <v-btn @click="openDialog()" prepend-icon="mdi-plus" variant="tonal">Добавить</v-btn>
    </v-app-bar>

    <v-main>
      <v-container>
        <v-row class="mb-4">
          <v-col cols="12" md="8">
            <v-text-field
              v-model="search"
              label="Поиск по модели..."
              prepend-inner-icon="mdi-magnify"
              clearable
              density="compact"
              variant="outlined"
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-select
              v-model="filterBrand"
              :items="name"
              label="Фильтр по марке"
              clearable
              density="compact"
              variant="outlined"
            ></v-select>
          </v-col>
        </v-row>

        <v-row>
          <v-col v-for="car in filteredCars" :key="car.id" cols="12" sm="6" md="4">
            <CarCard :car="car" @edit="openDialog" @delete="deleteCar" />
          </v-col>
        </v-row>

        <v-alert v-if="filteredCars.length === 0" type="info" variant="tonal" class="mt-4">
          Автомобили не найдены
        </v-alert>
      </v-container>
    </v-main>

    <CarFormDialog v-model="dialog" :car="editingCar" @save="saveCar" />
  </v-app>
</template>