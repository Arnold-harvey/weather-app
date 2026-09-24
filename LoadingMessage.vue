<script>
export default {
  name: 'LoadingMessage',
  props: {
    city: {
      type: String,
      default: ''
    }
  }
}
</script>

<template>
  <div class="loading" v-show="city">
    <div class="spinner"></div>
    <p>Chargement de la météo pour <strong>{{ city }}</strong>...</p>
  </div>
</template>

<style scoped>
.loading {
  text-align: center;
  padding: 2rem;
  color: #555;
}
.spinner {
  width: 40px;
  height: 40px;
  margin: 0 auto 1rem;
  border: 4px solid #e0e0e0;
  border-top-color: #3498db;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}
@keyframes spin {
  to { transform: rotate(360deg); }
}
</style>