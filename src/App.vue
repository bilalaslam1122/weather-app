<template>
  <div id="app">
    <button @click="openModal">Select City</button>
    <SearchModal
      v-if="showModal"
      @city-selected="selectCity"
      @close="closeModal"
    />
    <TemperatureDisplay
      v-if="currentTemperature"
      :city="city"
      :country="country"
      :temperature="currentTemperature"
      :dateTime="dateTime"
    />
    <TemperatureHistory :history="temperatureHistory" />
  </div>
</template>

<script>
import axios from "axios";
import SearchModal from "./components/SearchModal.vue"; // Assuming you create Modal.vue
import TemperatureDisplay from "./components/TemperatureDisplay.vue";
import TemperatureHistory from "./components/TemperatureHistory.vue";

import settings from "./config/settings.json";

export default {
  components: {
    SearchModal,
    TemperatureDisplay,
    TemperatureHistory,
  },
  data() {
    return {
      city: "",
      country: "",
      currentTemperature: null,
      dateTime: "",
      temperatureHistory: [],
      showModal: false,
    };
  },
  methods: {
    openModal() {
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
    },
    async selectCity(city) {
      this.city = city.name;
      this.country = city.country;
      this.temperatureHistory = [];
      this.fetchTemperatureData(city.id);
      setInterval(
        () => this.fetchTemperatureData(city.id),
        settings.updateInterval * 60000
      );
      this.closeModal(); // Close modal after city selection
    },
    async fetchTemperatureData(cityId) {
      try {
        const response = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?id=${cityId}&units=metric&appid=${settings.apiKey}`
        );
        const temperature = response.data.main.temp;
        const dateTime = new Date().toLocaleString();
        this.currentTemperature = temperature;
        this.dateTime = dateTime;
        this.temperatureHistory.push({ temperature, dateTime });
      } catch (error) {
        console.error("Error fetching temperature data:", error);
      }
    },
  },
};
</script>
<style scoped>
#app {
  font-family: "Arial", sans-serif;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #f3f5f6;
  min-height: calc(100dvh);
}

#app button {
  margin-top: 20px;
  background-color: #2c3e50;
  font-size: 18px;
  font-weight: 800;
  letter-spacing: 0.10000000149011612px;
  text-align: center;
  border-radius: 10px;
  border: none;
  color: white;
  padding: 8px 12px;
}
.city-input,
.city-selection,
.temperature-display,
.temperature-history {
  width: 80%;
  max-width: 600px;
  margin: 20px 0;
  padding: 20px;
  border-radius: 8px;
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.temperature-display {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.temperature-history {
  margin-top: 40px;
  width: 100%;
}

@media (max-width: 600px) {
  .city-input,
  .city-selection,
  .temperature-display,
  .temperature-history {
    width: 100%;
    margin: 10px 0;
    padding: 15px;
  }
}
</style>
