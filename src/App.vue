<template>
  <div class="container">
    <div class="search-bar">
      <input type="text" v-model="query" @keypress="fetchWeather" />
    </div>
    <div class="weather-container" v-if="Object.keys(weather).length > 0">
      <div class="current-weather">
        <h1 class="name">
          {{ weather.city.name }}, {{ weather.city.country }}
        </h1>
        <div class="indicator-container">
          <img
            class="weather-icon"
            :src="getIcon(weather.list[0].weather[0].icon)"
            alt="symbol"
          />
          <p class="temperature">{{ weather.list[0].main.temp }}°C</p>
          <div class="other-info">
            <p class="time">
              Time:
              <span>{{ formatDate(weather.list[0].dt) }}</span>
            </p>
            <p class="feels-like">
              Feels like: <span>{{ weather.list[0].main.feels_like }}°C</span>
            </p>
            <p class="humidity">
              Humidity: <span>{{ weather.list[0].main.humidity }}%</span>
            </p>
            <p class="pressure">
              Pressure: <span>{{ weather.list[0].main.pressure }}</span>
            </p>
            <p class="summary">
              Summary: <span>{{ weather.list[0].weather[0].description }}</span>
            </p>
          </div>
        </div>
      </div>
    </div>
    <div class="other-items-container">
      <div class="other-item" v-for="(w, index) in weather.list" :key="index">
        <weather-item
          :weather="w"
          :icon="getIcon(w.weather[0].icon)"
          :time="formatDate(w.dt)"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import WeatherItem from "./components/WeatherItem.vue";

export default {
  components: { WeatherItem },
  name: "App",
  data() {
    return {
      api_key: import.meta.env.VITE_API_KEY,
      base_url: import.meta.env.VITE_URL_BASE,
      weather: {},
      query: "",
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        axios({
          method: "get",
          url: `${this.base_url}forecast?q=${this.query}&cnt=5&units=metric&APPID=${this.api_key}`,
        })
          .then((res) => res)
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results.data;
    },
  },
  computed: {
    formatDate() {
      return (d) => {
        var day = new Date(d * 1000);
        return day.toDateString();
      };
    },
    getIcon() {
      return (icon) => {
        var iconCode = "https://openweathermap.org/img/w/" + icon + ".png";
        return iconCode;
      };
    },
  },
};
</script>
<style lang="scss">
* {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #ffffff;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='100' height='18' viewBox='0 0 100 18'%3E%3Cpath fill='%23e5e5f7' fill-opacity='0.59' d='M61.82 18c3.47-1.45 6.86-3.78 11.3-7.34C78 6.76 80.34 5.1 83.87 3.42 88.56 1.16 93.75 0 100 0v6.16C98.76 6.05 97.43 6 96 6c-9.59 0-14.23 2.23-23.13 9.34-1.28 1.03-2.39 1.9-3.4 2.66h-7.65zm-23.64 0H22.52c-1-.76-2.1-1.63-3.4-2.66C11.57 9.3 7.08 6.78 0 6.16V0c6.25 0 11.44 1.16 16.14 3.42 3.53 1.7 5.87 3.35 10.73 7.24 4.45 3.56 7.84 5.9 11.31 7.34zM61.82 0h7.66a39.57 39.57 0 0 1-7.34 4.58C57.44 6.84 52.25 8 46 8S34.56 6.84 29.86 4.58A39.57 39.57 0 0 1 22.52 0h15.66C41.65 1.44 45.21 2 50 2c4.8 0 8.35-.56 11.82-2z'%3E%3C/path%3E%3C/svg%3E");
  overflow: none;
  color: #fff;
}
.container {
  height: 80vh;
  width: 70vw;
  border-radius: 2rem;
  overflow: hidden;
  background-image: linear-gradient(to right, #111340, #2a2950);
  .search-bar {
    input {
      width: 40vw;
      outline: none;
      border: none;
      padding: 10px;
      padding-left: 20px;
      border-radius: 20px;
      margin-top: 20px;
      font-size: 18px;
    }
  }
  .current-weather {
    margin-top: 40px;
    .indicator-container {
      display: flex;
      justify-content: space-evenly;
      width: 100%;
      .weather-icon {
        height: 8rem;
        width: 8rem;
        object-fit: cover;
        margin: auto 0;
      }
      .temperature {
        font-size: 4rem;
        font-weight: 800;
        margin: auto 0;
      }
      .other-info {
        > p {
          font-weight: 800;
          font-size: 18px;
          line-height: 30px;
          text-align: start;
          span {
            font-weight: 400;
            font-size: 16px;
          }
        }
        .summary {
          text-transform: capitalize;
        }
      }
    }
  }
  .other-items-container {
    display: flex;
    justify-content: space-between;
    margin: 3rem;
    .info {
      > p {
        margin: 10px 0;
        &.time {
          font-weight: 800;
        }
      }
    }
  }
  @media screen and (max-width: 360px) {
    width: 100vw;
    height: 100vh;
    border-radius: 0;
    overflow-y: scroll;
    .search-bar {
      input {
        width: 90vw;
        border-radius: 10px;
        padding: 10px;
        font-size: initial;
      }
    }
    .current-weather {
      .indicator-container {
        flex-direction: column;
        justify-content: center;
        align-items: center;
        .weather-icon {
          margin-top: 20px;
        }
        .temperature {
          margin: 3rem 0;
        }
        .other-info {
          > p {
            text-align: center;
          }
        }
      }
    }
    .other-items-container {
      flex-direction: column;
      .other-item {
        margin: 2rem 0;
        img {
          height: 5rem;
          width: 5rem;
          object-fit: cover;
        }
      }
    }
  }
}
</style>
