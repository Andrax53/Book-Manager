<template>
  <div class="container mt-5">
    <header class="text-center mb-5 p-4 rounded-4 text-white" style="background: linear-gradient(135deg, #667eea, #764ba2);">
      <h1 class="display-4">Менеджер книг</h1>
      <p class="lead">Vue 3 + Bootstrap 5</p>
    </header>

    <AddBookForm @add-book="addBook" />

    <BookFilters
        v-model:searchQuery="searchQuery"
        v-model:filter="currentFilter"
        :books="books"
    />

    <!-- Сортировка (новое из практики 15) -->
    <div class="row mb-4">
      <div class="col-md-4">
        <select v-model="sortType" class="form-select form-select-lg">
          <option value="newest">Новые сверху</option>
          <option value="title">По названию (A-Z)</option>
          <option value="rating">По рейтингу (высокий сначала)</option>
        </select>
      </div>
    </div>

    <div v-if="displayedBooks.length === 0" class="alert alert-info text-center">
      Книги не найдены. Добавьте первую книгу!
    </div>

    <div class="row">
      <div v-for="book in displayedBooks" :key="book.id" class="col-lg-4 col-md-6 mb-4">
        <BookCard
            :book="book"
            @toggle="toggleBook(book.id)"
            @delete="deleteBook(book.id)"
            @rate="rateBook(book.id, $event)"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import {ref, computed, watch} from 'vue'
import AddBookForm from './components/AddBookForm.vue'
import BookFilters from './components/BookFilters.vue'
import BookCard from './components/BookCard.vue'

const books = ref([])
const saved = localStorage.getItem('books')
if (saved) books.value = JSON.parse(saved)

const currentFilter = ref('all')
const searchQuery = ref('')
const sortType = ref('newest')

watch(books, (newVal) => {
  localStorage.setItem('books', JSON.stringify(newVal))
}, {deep: true})

const addBook = (bookData) => {
  books.value.push({
    id: Date.now(),
    ...bookData,
    completed: false,
    rating: 0
  })
}

const toggleBook = (id) => {
  const book = books.value.find(b => b.id === id)
  if (book) {
    book.completed = !book.completed
    if (!book.completed) book.rating = 0
  }
}

const rateBook = (id, rating) => {
  const book = books.value.find(b => b.id === id)
  if (book && book.completed) book.rating = rating
}

const deleteBook = (id) => {
  if (confirm('Удалить книгу?')) {
    books.value = books.value.filter(b => b.id !== id)
  }
}

// Фильтрация (из 14)
const filteredBooks = computed(() => {
  return books.value.filter(book => {
    if (currentFilter.value === 'unread') return !book.completed
    if (currentFilter.value === 'read') return book.completed
    return true
  }).filter(book => {
    if (!searchQuery.value) return true
    const q = searchQuery.value.toLowerCase()
    return book.title.toLowerCase().includes(q) ||
        book.author.toLowerCase().includes(q)
  })
})

// Сортировка (новое из 15)
const displayedBooks = computed(() => {
  let list = [...filteredBooks.value]
  if (sortType.value === 'title') {
    list.sort((a, b) => a.title.localeCompare(b.title))
  } else if (sortType.value === 'rating') {
    list.sort((a, b) => (b.rating || 0) - (a.rating || 0))
  } else {
    list.sort((a, b) => b.id - a.id) // новые сверху
  }
  return list
})
</script>