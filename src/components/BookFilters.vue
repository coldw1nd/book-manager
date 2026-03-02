<script setup>
import { computed } from 'vue'
const props = defineProps(['filter', 'books'])
defineEmits(['update:filter'])

const searchQuery = defineModel('searchQuery')

const filterOptions = [
  { value: 'all', label: 'Все' },
  { value: 'unread', label: 'Непрочитанные' },
  { value: 'read', label: 'Прочитанные' }
]

const total = computed(() => props.books.length)
const completed = computed(() => props.books.filter(b => b.completed).length)
</script>

<template>
  <div class="filters">
    <div class="search">
      <input v-model="searchQuery" type="text" placeholder="Поиск по названию или автору..." />
    </div>
    <div class="filter-buttons">
      <button v-for="opt in filterOptions" :key="opt.value" 
        @click="$emit('update:filter', opt.value)"
        :class="['filter-btn', { active: filter === opt.value }]">
        {{ opt.label }}
      </button>
    </div>
    <div class="stats">
      <p>Всего: {{ total }} | Прочитано: {{ completed }} | Осталось: {{ total - completed }}</p>
    </div>
  </div>
</template>

<style scoped>
.filters { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); margin-bottom: 20px; }
.search input { width: 100%; padding: 10px; border: 1px solid #ddd; border-radius: 4px; margin-bottom: 15px; }
.filter-buttons { display: flex; gap: 10px; margin-bottom: 15px; }
.filter-btn { padding: 8px 16px; border: 1px solid #ddd; background: white; border-radius: 4px; cursor: pointer; }
.filter-btn.active { background: #4CAF50; color: white; border-color: #4CAF50; }
.stats { padding-top: 15px; border-top: 1px solid #eee; color: #666; font-size: 0.9em; }
</style>