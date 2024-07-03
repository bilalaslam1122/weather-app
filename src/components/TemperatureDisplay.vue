<template>
  <div class="temperature-display-container">
    <h2>{{ city }}, {{ country }}</h2>
    <p class="temperature-info">Temperature: {{ temperature }}°C</p>
    <p class="temperature-info">Date/Time: {{ dateTime }}</p>
  </div>
</template>

<script>
export default {
  props: ["city", "country", "temperature", "dateTime"],
};
</script>

<style scoped>
.temperature-display-container {
  max-width: 400px;
  margin: 20px auto;
  padding: 20px;
  background-color: #ffffff;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h2 {
  margin-bottom: 10px;
  font-size: 24px;
  color: #333333;
}

.temperature-info {
  font-size: 18px;
  color: #666666;
  margin-bottom: 5px;
}
</style>
