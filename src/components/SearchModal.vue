<template>
  <transition name="modal">
    <div class="modal">
      <div class="modal-content">
        <header>
          <img
            class="icon-cancel pointer"
            :src="IconCancel"
            alt="Cancel Icon"
            @click="closeModal"
          />
        </header>

        <main>
          <div class="input-bar">
            <input
              type="text"
              v-model="searchQuery"
              placeholder="Enter city name"
              @input="searchCities"
            />
            <img :src="IconSearch" class="pointer" @click="searchCities" />
          </div>

          <ul v-if="cityOptions.length">
            <li
              v-for="city in cityOptions"
              :key="city.id"
              @click="selectCity(city)"
            >
              {{ city.name }}, {{ city.country }}
            </li>
          </ul>
        </main>
      </div>
    </div>
  </transition>
</template>

<script>
import axios from "axios";
import IconCancel from "../assets/cancel.png";
import IconSearch from "../assets/search.png";
import settings from "../config/settings.json";

export default {
  name: "SearchModal",
  data() {
    return {
      IconCancel,
      IconSearch,
      searchQuery: "",
      cityOptions: [],
      debounceTimeout: null,
    };
  },
  methods: {
    async searchCities() {
      clearTimeout(this.debounceTimeout);
      this.debounceTimeout = setTimeout(async () => {
        try {
          const response = await axios.get(
            `https://api.openweathermap.org/data/2.5/find?q=${this.searchQuery}&appid=${settings.apiKey}`
          );
          this.cityOptions = response.data.list.map((city) => ({
            name: city.name,
            country: city.sys.country,
            id: city.id,
          }));
        } catch (error) {
          this.cityOptions = [];
          console.error("Error fetching city data:", error);
        }
      }, 300);
    },
    selectCity(city) {
      this.$emit("city-selected", city);
      this.closeModal();
    },
    closeModal() {
      this.$emit("close");
    },
  },
};
</script>
<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: flex-start;
  transition: opacity 0.3s ease;
}

.modal-content {
  margin-top: 50px;
  width: 20%;
  height: fit-content;
  border-radius: 20px;
  background-color: #ffffff;
  padding: 10px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, opacity 0.3s ease;
  min-width: 325px;
}

header {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 12px;
}
header img {
  width: 24px;
  height: 24px;
}
.input-bar {
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #f3f4f6;
  padding: 12px;
  border-radius: 10px;
}
.input-bar input {
  width: 100%;
  outline: none;
  background-color: transparent;
  border: none;
  font-size: 16px;
}

.pointer {
  cursor: pointer;
}
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;
}

.modal-enter,
.modal-leave-to {
  opacity: 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  cursor: pointer;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

button {
  margin-top: 10px;
}
</style>
