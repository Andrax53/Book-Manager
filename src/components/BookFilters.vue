<template>
  <div class="card p-4 shadow mb-4">
    <div class="row g-3">
      <div class="col-12">
        <input v-model="searchQuery"
               type="text"
               class="form-control form-control-lg"
               placeholder="Поиск по названию или автору...">
      </div>

      <div class="col-12">
        <div class="btn-group w-100" role="group">
          <button v-for="option in filterOptions" :key="option.value"
                  @click="$emit('update:filter', option.value)"
                  class="btn"
                  :class="filter === option.value ? 'btn-primary' : 'btn-outline-primary'">
            {{ option.label }}
          </button>
        </div>
      </div>

      <div class="col-12 text-center text-muted">
        Всего: {{ total }} | Прочитано: {{ completed }} | Осталось: {{ total - completed }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps(['filter', 'books'])
const emit = defineEmits(['update:filter'])
const searchQuery = defineModel('searchQuery')

const filterOptions = [
  { value: 'all', label: 'Все' },
  { value: 'unread', label: 'Непрочитанные' },
  { value: 'read', label: 'Прочитанные' }
]

const total = computed(() => props.books.length)
const completed = computed(() => props.books.filter(b => b.completed).length)
</script>