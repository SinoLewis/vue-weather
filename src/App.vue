<template>
  <div
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16
        ? 'bg-gradient-to-b from-red-900 via-yellow-500 to-red-100'
        : 'bg-gradient-to-b from-blue-900 via-green-500 to-blue-100'
    "
  >
    <div class="flex flex-col items-center space-y-6 h-screen p-4 text-white">
      <div class="flex w-screen justify-center text-black">
        <input
          class="
            w-3/5
            h-12
            m-4
            p-4
            rounded-tl-lg rounded-br-lg
            border-2 border-black
          "
          type="search"
          placeholder="Enter place"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="flex flex-col justify-center items-center p-4 space-y-2">
            <h1 class="font-semibold text-3xl">
              {{ weather.name }}, {{ weather.sys.country }}
            </h1>
            <p class="italic text-lg">{{ dateBuilder() }}</p>
          </div>
        </div>

        <div class="weather-box">
          <div class="">
            <h1 class="font-bold text-9xl p-4">
              {{ Math.round(weather.main.temp) }}°c
            </h1>
          </div>
          <div class="font-semibold text-6xl p-4">
            <h1>{{ weather.weather[0].main }}</h1>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "023a2f36813a5540d3ae625b2bded138",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
</style>
