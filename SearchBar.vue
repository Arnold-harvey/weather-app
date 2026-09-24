<script>
export default {
  name: 'SearchBar',
  props: {
    modelValue: {
      type: String,
      default: ''
    },
    isLoading: {
      type: Boolean,
      default: false
    }
  },
  emits: ['update:modelValue', 'search'],
  methods: {
    onInput(event) {
      this.$emit('update:modelValue', event.target.value)
    },
    onSubmit() {
      this.$emit('search')
    }
  }
}
</script>

<template>
  <div class="search-bar">
    <input
      type="text"
      :value="modelValue"
      @input="onInput"
      @keyup.enter="onSubmit"
      placeholder="Entrez une ville (ex: Bujumbura, Paris...)"
      :disabled="isLoading"
    />
    <button @click="onSubmit" :disabled="isLoading">
      {{ isLoading ? 'Recherche...' : 'Rechercher' }}
    </button>
  </div>
</template>

<style scoped>
.search-bar {
  display: flex;
  gap: 0.5rem;
  max-width: 500px;
  margin: 0 auto;
}
input {
  flex: 1;
  padding: 0.75rem 1rem;
  border: 2px solid #ddd;
  border-radius: 8px;
  font-size: 1rem;
  outline: none;
  transition: border-color 0.2s;
}
input:focus {
  border-color: #3498db;
}
button {
  padding: 0.75rem 1.5rem;
  background: #3498db;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s;
}
button:hover:not(:disabled) {
  background: #2980b9;
}
button:disabled {
  background: #95a5a6;
  cursor: not-allowed;
}
</style>