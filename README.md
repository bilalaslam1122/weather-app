# Weather App

This is a Vue 2 weather application that allows users to select a city and view its current temperature. The application uses the OpenWeatherMap API to fetch weather data and updates the temperature at specified intervals.

## Components

### 1. SearchModal.vue

The `SearchModal` component provides a modal for users to search for cities and select one from the search results.

- **Props**: None
- **Data**:
  - `searchQuery`: The current search query input by the user.
  - `cityOptions`: Array of city options returned from the API.
  - `debounceTimeout`: Timeout identifier for debouncing API requests.
- **Methods**:
  - `searchCities()`: Debounced method to fetch city data from the OpenWeatherMap API based on the search query.
  - `selectCity(city)`: Emits the selected city and closes the modal.
  - `closeModal()`: Emits the close event to hide the modal.
- **Template**:
  - Input field for city search.
  - List of city options for selection.

### 2. TemperatureDisplay.vue

The `TemperatureDisplay` component displays the current temperature of the selected city.

- **Props**:
  - `city`: Name of the selected city.
  - `country`: Country of the selected city.
  - `temperature`: Current temperature of the city.
  - `dateTime`: Date and time of the temperature reading.
- **Template**:
  - Displays city name, country, temperature, and date/time.

### 3. TemperatureHistory.vue

The `TemperatureHistory` component displays the historical temperature data of the selected city.

- **Props**:
  - `history`: Array of temperature history objects containing `dateTime` and `temperature`.
- **Template**:
  - List of historical temperature readings.

### 4. App.vue

The `App` component is the main application component that orchestrates the other components.

- **Components**:
  - `SearchModal`
  - `TemperatureDisplay`
  - `TemperatureHistory`
- **Data**:
  - `city`: Name of the selected city.
  - `country`: Country of the selected city.
  - `currentTemperature`: Current temperature of the selected city.
  - `dateTime`: Date and time of the current temperature reading.
  - `temperatureHistory`: Array of historical temperature data.
  - `showModal`: Boolean to control the visibility of the `SearchModal`.
- **Methods**:
  - `openModal()`: Shows the `SearchModal`.
  - `closeModal()`: Hides the `SearchModal`.
  - `selectCity(city)`: Sets the selected city and fetches its temperature data.
  - `fetchTemperatureData(cityId)`: Fetches the temperature data for the given city ID and updates the temperature history.
- **Template**:
  - Button to open the city selection modal.
  - `SearchModal` for city selection.
  - `TemperatureDisplay` for displaying the current temperature.
  - `TemperatureHistory` for displaying the temperature history.

## Setup and Usage

1. **Clone the repository**:

   ```sh
   git clone https://github.com/bilalaslam1122/weather-app.git
   cd weather-app
   ```

2. **Install dependencies**:

   ```sh
   npm install
   ```

3. **Configure API settings**:

   Create a `config/settings.json` file with the following content:

   ```json
   {
     "apiKey": "YOUR_OPENWEATHERMAP_API_KEY",
     "updateInterval": 10  
   }
   ```

4. **Run the application**:

   ```sh
   npm run serve
   ```

5. **Open the application**:

   Open your browser and navigate to `http://localhost:8080`.

## How It Works

- The user clicks the "Select City" button to open the `SearchModal`.
- In the `SearchModal`, the user enters a city name, and the application fetches matching cities from the OpenWeatherMap API.
- The user selects a city from the list, which triggers the `selectCity` method in `App.vue`.
- The application fetches the current temperature data for the selected city and updates the `TemperatureDisplay` and `TemperatureHistory` components.
- The temperature data is updated at the specified interval as defined in the `settings.json` file.

---
