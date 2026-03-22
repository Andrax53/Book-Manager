<template>
  <form @submit.prevent="handleSubmit" class="card p-4 shadow mb-4">
    <h4 class="card-title mb-3">Добавить новую книгу</h4>
    <div class="row g-3">
      <div class="col-md-6">
        <label class="form-label">Название</label>
        <input v-model="formData.title" class="form-control" required>
      </div>
      <div class="col-md-6">
        <label class="form-label">Автор</label>
        <input v-model="formData.author" class="form-control" required>
      </div>
      <div class="col-md-6">
        <label class="form-label">Жанр</label>
        <select v-model="formData.genre" class="form-select" required>
          <option value="">Выберите жанр</option>
          <option value="Роман">Роман</option>
          <option value="Фантастика">Фантастика</option>
          <option value="Детектив">Детектив</option>
          <option value="Научная">Научная</option>
          <option value="Поэзия">Поэзия</option>
        </select>
      </div>
      <div class="col-12">
        <label class="form-label">URL обложки (фото)</label>
        <input v-model="formData.photo" type="url" class="form-control" placeholder="https://...">
      </div>
      <div class="col-12">
        <label class="form-label">Описание</label>
        <textarea v-model="formData.description" class="form-control" rows="3" placeholder="Краткое описание..."></textarea>
      </div>
      <div class="col-12">
        <button type="submit" class="btn btn-success w-100">Добавить книгу</button>
      </div>
    </div>
  </form>
</template>

<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['add-book'])

const formData = reactive({
  title: '',
  author: '',
  genre: '',
  photo: '',
  description: ''
})

const handleSubmit = () => {
  emit('add-book', { ...formData })
  formData.title = ''
  formData.author = ''
  formData.genre = ''
  formData.photo = ''
  formData.description = ''
}
</script>