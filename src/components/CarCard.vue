<template>
  <!-- Добавлен класс car-card -->
  <v-card class="car-card mx-auto my-2" elevation="0" rounded="lg">
    
    <!-- Добавлен класс image-wrapper -->
    <div class="image-wrapper">
      <v-img
        :src="car.image || 'https://placehold.co/600x400/333/fff?text=No+Image'"
        cover
        class="align-end text-white"
      >
        <!-- Добавлен класс image-overlay -->
        <div class="image-overlay"></div>
        
        <!-- Добавлен класс year-badge -->
        <v-chip 
          size="small" 
          class="year-badge font-weight-bold" 
          variant="flat"
        >
          {{ car.year }}
        </v-chip>
      </v-img>
    </div>

    <v-card-title class="pt-4 pb-1 text-h6 font-weight-bold">
      {{ car.name }}
    </v-card-title>
    
    <v-card-subtitle class="pb-2 text-grey-darken-1">
      {{ car.brand }}
    </v-card-subtitle>

    <v-card-text class="pt-0">
      <div class="d-flex align-center justify-space-between mt-2">
        <!-- Добавлен класс price-tag -->
        <span class="price-tag">{{ car.price.toLocaleString('ru-RU') }} ₽</span>
      </div>
    </v-card-text>

    <v-divider class="mx-4 mb-2"></v-divider>

    <v-card-actions class="px-4 pb-4">
      <v-btn 
        variant="tonal" 
        size="small" 
        prepend-icon="mdi-pencil" 
        @click="$emit('edit', car)"
        class="action-btn"
      >
        Изменить
      </v-btn>
      <v-spacer></v-spacer>
      <!-- Добавлен класс delete-btn -->
      <v-btn 
        icon="mdi-delete" 
        size="small" 
        color="error" 
        variant="text" 
        @click="$emit('delete', car.id)"
        class="delete-btn"
      ></v-btn>
    </v-card-actions>
  </v-card>
</template>

<script setup>
defineProps(['car'])
defineEmits(['edit', 'delete'])
</script>
<!-- Тег style удален -->