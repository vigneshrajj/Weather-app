<template>
  <img :src="icon" alt="symbol" />
  <div class="info">
    <p class="time">
      {{ time }}
    </p>
    <p class="high">
      High: <span>{{ weather.main.temp_max }}</span>
    </p>
    <p class="low">
      Low: <span>{{ weather.main.temp_min }}</span>
    </p>
  </div>
</template>

<script>
export default {
  name: "WeatherItem",
  props: {
    weather: Object,
    icon: String,
    time: String,
  },
};
</script>