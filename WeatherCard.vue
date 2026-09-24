<script>
export default {
  name: 'WeatherCard',
  props: {
    weather: {
      type: Object,
      required: true
    }
  },
  computed: {
    // Mappe le code météo Open-Meteo vers une icône emoji
    weatherIcon() {
      const code = this.weather.weathercode
      if (code === 0) return '☀️'
      if (code <= 2) return '🌤️'
      if (code === 3) return '☁️'
      if (code >= 45 && code <= 48) return '🌫️'
      if (code >= 51 && code <= 67) return '🌧️'
      if (code >= 71 && code <= 77) return '❄️'
      if (code >= 80 && code <= 82) return '🌧️'
      if (code >= 95) return '⛈️'
      return '🌡️'
    },
    weatherDescription() {
      const code = this.weather.weathercode
      if (code === 0) return 'Ciel dégagé'
      if (code <= 2) return 'Partiellement nuageux'
      if (code === 3) return 'Couvert'
      if (code >= 45 && code <= 48) return 'Brouillard'
      if (code >= 51 && code <= 67) return 'Pluie'
      if (code >= 71 && code <= 77) return 'Neige'
      if (code >= 80 && code <= 82) return 'Averses'
      if (code >= 95) return 'Orage'
      return 'Inconnu'
    }
  }
}
</script>

<template>
  <div class="weather-card">
    <h2>{{ weather.city }}</h2>
    <div class="icon">{{ weatherIcon }}</div>
    <p class="description">{{ weatherDescription }}</p>

    <div class="details">
      <div class="detail">
        <span class="label">🌡️ Température</span>
        <span class="value">{{ weather.temperature }} °C</span>
      </div>
      <div class="detail">
        <span class="label">💧 Humidité</span>
        <span class="value">{{ weather.humidity }} %</span>
      </div>
      <div class="detail">
        <span class="label">💨 Vent</span>
        <span class="value">{{ weather.windspeed }} km/h</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.weather-card {
  max-width: 400px;
  margin: 2rem auto;
  padding: 2rem;
  background: linear-gradient(135deg, #74ebd5, #acb6e5);
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
  text-align: center;
  color: #2c3e50;
}
h2 {
  margin: 0 0 1rem;
  font-size: 1.8rem;
}
.icon {
  font-size: 5rem;
  line-height: 1;
}
.description {
  font-size: 1.1rem;
  font-weight: 600;
  margin: 0.5rem 0 1.5rem;
}
.details {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  background: rgba(255, 255, 255, 0.5);
  padding: 1rem;
  border-radius: 12px;
}
.detail {
  display: flex;
  justify-content: space-between;
  font-size: 1rem;
}
.label { font-weight: 500; }
.value { font-weight: 700; }
</style>