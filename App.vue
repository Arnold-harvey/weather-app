<script>
import SearchBar from './components/SearchBar.vue'
import WeatherCard from './components/WeatherCard.vue'
import LoadingMessage from './components/LoadingMessage.vue'
import SearchHistory from './components/SearchHistory.vue'

export default {
  name: 'App',
  components: {
    SearchBar,
    WeatherCard,
    LoadingMessage,
    SearchHistory
  },
  data() {
    return {
      city: '',
      weather: null,
      isLoading: false,
      errorMessage: '',
      searchedCity: '',
      history: []
    }
  },
  methods: {
    async searchWeather() {
      this.errorMessage = ''
      this.weather = null

      if (!this.city.trim()) {
        this.errorMessage = "Veuillez entrer le nom d'une ville."
        return
      }

      this.isLoading = true
      this.searchedCity = this.city.trim()

      try {
        const geoUrl = `https://geocoding-api.open-meteo.com/v1/search?name=${encodeURIComponent(this.searchedCity)}&count=1&language=fr`
        const geoRes = await fetch(geoUrl)

        if (!geoRes.ok) throw new Error('Erreur réseau lors du géocoding')

        const geoData = await geoRes.json()

        if (!geoData.results || geoData.results.length === 0) {
          throw new Error('CITY_NOT_FOUND')
        }

        const { latitude, longitude, name, country } = geoData.results[0]

        const weatherUrl = `https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&current=temperature_2m,relative_humidity_2m,wind_speed_10m,weather_code`
        const weatherRes = await fetch(weatherUrl)

        if (!weatherRes.ok) throw new Error('Erreur réseau lors de la météo')

        const weatherData = await weatherRes.json()
        const c = weatherData.current

        this.weather = {
          city: `${name}${country ? ', ' + country : ''}`,
          temperature: c.temperature_2m,
          humidity: c.relative_humidity_2m,
          windspeed: c.wind_speed_10m,
          weathercode: c.weather_code
        }

        // Ajout à l'historique (sans doublons)
        if (!this.history.includes(this.searchedCity)) {
          this.history.unshift(this.searchedCity)
          // Garde les 8 dernières recherches
          if (this.history.length > 8) {
            this.history.pop()
          }
        }
      } catch (err) {
        if (err.message === 'CITY_NOT_FOUND') {
          this.errorMessage = `Ville "${this.searchedCity}" introuvable. Vérifiez l'orthographe.`
        } else {
          this.errorMessage = 'Une erreur est survenue. Vérifiez votre connexion et réessayez.'
        }
      } finally {
        this.isLoading = false
      }
    },
    searchFromHistory(cityName) {
      this.city = cityName
      this.searchWeather()
    },
    clearHistory() {
      this.history = []
    }
  }
}
</script>

<template>
  <div class="app">
    <header>
      <h1>🌤️ Météo App</h1>
      <p>Cherchez la météo actuelle d'une ville</p>
    </header>

    <SearchBar v-model="city" :is-loading="isLoading" @search="searchWeather" />

    <LoadingMessage :city="searchedCity" v-if="isLoading" />

    <div class="error" v-else-if="errorMessage">
      ⚠️ {{ errorMessage }}
    </div>

    <WeatherCard v-else-if="weather" :weather="weather" />

    <div class="welcome" v-else>
      <p>Entrez le nom d'une ville pour commencer.</p>
      <p class="hint">Exemples : Bujumbura, Tokyo, Nairobi, Shanghai, Paris</p>
    </div>

    <SearchHistory
      :history="history"
      @select="searchFromHistory"
      @clear="clearHistory"
    />
  </div>
</template>

<style scoped>
.app {
  min-height: 100vh;
  background: #f0f4f8;
  padding: 2rem 1rem;
  font-family: 'Segoe UI', Tahoma, sans-serif;
}
header {
  text-align: center;
  margin-bottom: 2rem;
  color: #2c3e50;
}
header h1 {
  font-size: 2.5rem;
  margin: 0 0 0.5rem;
}
header p {
  margin: 0;
  color: #7f8c8d;
}
.error {
  max-width: 400px;
  margin: 2rem auto;
  padding: 1rem 1.5rem;
  background: #ffe0e0;
  color: #c0392b;
  border-radius: 12px;
  text-align: center;
  font-weight: 600;
}
.welcome {
  text-align: center;
  margin-top: 3rem;
  color: #7f8c8d;
}
.hint {
  font-size: 0.9rem;
  font-style: italic;
}
</style>