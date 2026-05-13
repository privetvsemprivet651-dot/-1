<template>
  <v-dialog v-model="open" max-width="500" persistent>
    <v-card>
      <!-- Добавлен класс dialog-header -->
      <v-card-title class="dialog-header d-flex align-center">
        <v-icon start>mdi-car</v-icon>
        {{ form.id ? 'Редактирование' : 'Новый автомобиль' }}
      </v-card-title>
      
      <v-card-text>
        <!-- Добавлен класс custom-input к форме или полям -->
        <v-form @submit.prevent="submit" class="pt-2 custom-input">
          
          <v-select
            v-model="form.name" 
            :items="['Toyota', 'BMW', 'Mercedes', 'Audi', 'Lada', 'Kia', 'Hyundai']"
            label="Модель" 
            required 
            variant="outlined" 
            density="compact" 
            class="mb-2"
          ></v-select>
          
          <v-text-field
            v-model="form.brand"
            label="Марка"
            required
            variant="outlined"
            density="compact"
            class="mb-2"
          ></v-text-field>

          <v-row>
            <v-col cols="6">
              <v-text-field 
                v-model.number="form.year" 
                label="Год" 
                type="number" 
                required 
                variant="outlined" 
                density="compact"
              ></v-text-field>
            </v-col>
            <v-col cols="6">
              <v-text-field 
                v-model.number="form.price" 
                label="Цена (₽)" 
                type="number" 
                required 
                variant="outlined" 
                density="compact"
              ></v-text-field>
            </v-col>
          </v-row>

          <v-text-field 
            v-model="form.image" 
            label="URL изображения" 
            variant="outlined" 
            density="compact"
          ></v-text-field>

          <v-card-actions class="px-0 mt-2">
            <v-spacer></v-spacer>
            <v-btn variant="text" @click="open = false">Отмена</v-btn>
            <v-btn type="submit" color="primary" variant="tonal" prepend-icon="mdi-check">Сохранить</v-btn>
          </v-card-actions>
        </v-form>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<script setup>
import { computed, watch, ref } from 'vue'

const props = defineProps({
  modelValue: Boolean,
  car: Object
})
const emit = defineEmits(['update:modelValue', 'save'])

const form = ref({})
const open = computed({
  get: () => props.modelValue,
  set: (val) => emit('update:modelValue', val)
})

watch(() => props.car, (val) => {
  form.value = val ? { ...val } : {}
}, { immediate: true })

const submit = () => {
  emit('save', form.value)
}
</script>
<!-- Тег style удален -->