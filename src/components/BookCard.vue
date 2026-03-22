<template>
  <div class="card h-100 shadow">
    <img v-if="book.photo" :src="book.photo" class="card-img-top" style="height: 220px; object-fit: cover;">
    <div class="card-body d-flex flex-column">
      <h5 class="card-title">{{ book.title }}</h5>
      <h6 class="card-subtitle text-muted mb-2">{{ book.author }}</h6>
      <span class="badge bg-secondary mb-2">{{ book.genre }}</span>

      <p v-if="book.description" class="card-text flex-grow-1">{{ book.description }}</p>

      <div v-if="book.completed" class="rating mb-3">
        <span v-for="star in 5" :key="star" @click="$emit('rate', star)"
              class="star fs-3" style="cursor:pointer; color:gold;">
          {{ star <= book.rating ? '★' : '☆' }}
        </span>
      </div>
    </div>
    <div class="card-footer bg-transparent border-0">
      <button @click="$emit('toggle')"
              class="btn w-100 mb-2"
              :class="book.completed ? 'btn-outline-primary' : 'btn-success'">
        {{ book.completed ? 'Прочитано ✓' : 'Отметить прочитанной' }}
      </button>
      <button @click="$emit('delete')" class="btn btn-danger w-100">Удалить</button>
    </div>
  </div>
</template>

<script setup>
defineProps(['book'])
defineEmits(['toggle', 'delete', 'rate'])
</script>

<style scoped>
.star:hover { transform: scale(1.3); }
</style>