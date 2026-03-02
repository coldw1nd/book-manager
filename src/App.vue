<script setup>
import { ref, computed, watch } from 'vue'
import AddBookForm from './components/AddBookForm.vue'
import BookFilters from './components/BookFilters.vue'
import BookCard from './components/BookCard.vue'

const books = ref(JSON.parse(localStorage.getItem('books') || '[]'))
const currentFilter = ref('all')
const searchQuery = ref('')

watch(books, (newVal) => {
  localStorage.setItem('books', JSON.stringify(newVal))
}, { deep: true })

const addBook = (data) => {
  books.value.push({ id: Date.now(), ...data, completed: false, rating: 0, isFavorite: false })
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
  if (confirm('Удалить книгу?')) books.value = books.value.filter(b => b.id !== id)
}

const toggleFavorite = (id) => {
  const book = books.value.find(b => b.id === id)
  if (book) book.isFavorite = !book.isFavorite
}

const filteredBooks = computed(() => {
  return books.value
    .filter(b => {
      if (currentFilter.value === 'unread') return !b.completed
      if (currentFilter.value === 'read') return b.completed
      return true
    })
    .filter(b => {
      const q = searchQuery.value.toLowerCase()
      return b.title.toLowerCase().includes(q) || b.author.toLowerCase().includes(q)
    })
})
</script>

<template>
  <div class="app">
    <header>
      <h1>Менеджер книг</h1>
      <p>Твоя личная библиотека</p>
    </header>
    <main>
      <AddBookForm @add-book="addBook" />
      <BookFilters v-model:searchQuery="searchQuery" v-model:filter="currentFilter" :books="books" />
      
      <div v-if="filteredBooks.length === 0" class="empty">Книг не найдено</div>
      <div v-else class="list">
        <BookCard v-for="book in filteredBooks" :key="book.id" :book="book"
          @toggle="toggleBook(book.id)"
          @delete="deleteBook(book.id)"
          @rate="rateBook(book.id, $event)"
          @toggle-favorite="toggleFavorite(book.id)"
        />
      </div>
    </main>
  </div>
</template>

<style>
body { font-family: sans-serif; background: #f0f2f5; margin: 0; }
.app { max-width: 800px; margin: 0 auto; padding: 20px; }
header { text-align: center; margin-bottom: 30px; background: linear-gradient(135deg, #667eea, #764ba2); color: white; padding: 30px; border-radius: 12px; }
.empty { text-align: center; color: #999; margin-top: 40px; }
</style>