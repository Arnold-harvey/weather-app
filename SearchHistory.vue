<script>
export default {
  name: 'SearchHistory',
  props: {
    history: {
      type: Array,
      default: () => []
    }
  },
  emits: ['select', 'clear'],
  methods: {
    onSelect(city) {
      this.$emit('select', city)
    },
    onClear() {
      this.$emit('clear')
    }
  }
}
</script>

<template>
  <div class="history" v-if="history.length > 0">
    <div class="history-header">
      <h3>🕘 Historique des recherches</h3>
      <button class="clear-btn" @click="onClear">Effacer</button>
    </div>
    <ul>
      <li v-for="(cityItem, index) in history" :key="index" @click="onSelect(cityItem)">
        {{ cityItem }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
.history {
  max-width: 400px;
  margin: 1.5rem auto;
  padding: 1rem 1.5rem;
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}
.history-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.75rem;
}
.history-header h3 {
  margin: 0;
  font-size: 1rem;
  color: #2c3e50;
}
.clear-btn {
  background: transparent;
  border: 1px solid #ddd;
  color: #7f8c8d;
  padding: 0.25rem 0.75rem;
  border-radius: 6px;
  font-size: 0.85rem;
  cursor: pointer;
}
.clear-btn:hover {
  background: #ffe0e0;
  color: #c0392b;
  border-color: #c0392b;
}
ul {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}
li {
  padding: 0.4rem 0.9rem;
  background: #ecf0f1;
  border-radius: 20px;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.2s;
}
li:hover {
  background: #3498db;
  color: white;
}
</style>