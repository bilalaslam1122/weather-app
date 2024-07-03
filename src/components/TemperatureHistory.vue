<template>
  <div>
    <h3 v-if="history.length > 0">Temperature History</h3>
    <ul class="history-list">
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
<style scoped>
.history-list {
  max-height: 400px;
  overflow-y: auto;
}
</style>
