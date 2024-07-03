<template>
  <div>
    <h3 v-if="history.length > 0">Temperature History</h3>
    <ul>
      <li v-for="(reading, index) in history" :key="index">
        {{ reading.dateTime }} - {{ reading.temperature }}°C
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["history"],
};
</script>
