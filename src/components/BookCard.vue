<script setup>
defineProps(['book'])
defineEmits(['toggle', 'delete', 'rate', 'toggle-favorite'])
</script>

<template>
  <div class="book-card" :class="{ completed: book.completed }">
    <div class="book-info">
      <div class="title-row">
        <h3>{{ book.title }}</h3>
        <span @click="$emit('toggle-favorite')" class="fav-icon" :class="{ active: book.isFavorite }">
          {{ book.isFavorite ? '❤️' : '🤍' }}
        </span>
      </div>
      <p class="author">Автор: {{ book.author }}</p>
      <span class="genre">{{ book.genre }}</span>
    </div>

    <div class="book-actions">
      <div v-if="book.completed" class="rating">
        <span v-for="star in 5" :key="star" @click="$emit('rate', star)" class="star">
          {{ star <= book.rating ? '★' : '☆' }}
        </span>
      </div>

      <button @click="$emit('toggle')" :class="['btn', book.completed ? 'btn-secondary' : 'btn-primary']">
        {{ book.completed ? 'Прочитано' : 'Прочитать' }}
      </button>

      <button @click="$emit('delete')" class="btn btn-danger">✕</button>
    </div>
  </div>
</template>

<style scoped>
.book-card {
  background: white; border-radius: 8px; padding: 16px; margin-bottom: 12px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1); display: flex; justify-content: space-between; align-items: center;
}
.completed { background: #f0f7f0; opacity: 0.9; }
.book-info { flex: 1; }
.title-row { display: flex; align-items: center; gap: 10px; }
.fav-icon { cursor: pointer; font-size: 1.2rem; filter: grayscale(1); transition: 0.3s; }
.fav-icon.active { filter: grayscale(0); }
.author { color: #666; font-size: 0.9em; margin: 4px 0; }
.genre { background: #e0e0e0; padding: 2px 8px; border-radius: 4px; font-size: 0.8em; }
.book-actions { display: flex; gap: 10px; align-items: center; }
.star { color: gold; cursor: pointer; font-size: 1.2rem; }
.btn { padding: 8px 12px; border: none; border-radius: 4px; cursor: pointer; transition: 0.3s; }
.btn-primary { background: #4CAF50; color: white; }
.btn-secondary { background: #2196F3; color: white; }
.btn-danger { background: #f44336; color: white; }
</style>