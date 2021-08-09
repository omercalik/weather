<template>
  <div class="wrapper">
    <div class="visual-info">
      <img
        v-if="weatherImageSrc"
        :src="'/img/' + weatherImageSrc + '.png'"
        class="weather-image"
      />

      <p class="temperature">
        {{ Math.round(currentWeatherInfo.the_temp) }}
        <span class="unit">℃</span>
      </p>

      <p class="state-name">
        {{ currentWeatherInfo.weather_state_name }}
      </p>

      <p class="date">
        Bugün •
        {{
          new Date(currentWeatherInfo.created).toLocaleString("default", {
            day: "numeric",
            weekday: "long",
            month: "long",
          })
        }}
      </p>

      <img width="15" src="../assets/location.svg" />
      <span class="city-name">{{ weatherData.title }}</span>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue"

export default defineComponent({
  name: "MainInfo",

  props: ["weatherData"],
  data() {
    return {
      weatherImageSrc: "",
      currentWeatherInfo: {},
    }
  },

  mounted() {
    this.weatherImageSrc = this.weatherData.consolidated_weather[0].weather_state_name.replace(
      /\s/g,
      ""
    )
    this.currentWeatherInfo = this.weatherData.consolidated_weather[0]
    console.log(this.weatherData)
  },

  methods: {},
})
</script>

<style lang="scss">
.visual-info {
  height: 100vh;
  width: 25vw;
  background-color: #1e213a;
  text-align: center;

  .temperature {
    color: #e7e7eb;
    font-weight: 500;
    font-size: 140px;
  }

  .unit {
    font-size: 40px;
    font-weight: 100;
    opacity: 0.7;
    color: #e7e7eb;
    margin-left: -25px;
  }

  .state-name {
    color: #a09fb1;
    font-size: 36px;
    font-weight: 600;
    line-height: 42px;
  }

  .date {
    color: #88869d;
    font-weight: 500;
    font-size: 18px;
    line-height: 21px;
  }

  .city-name {
    font-weight: 600;
    font-size: 18px;
    color: #88869d;
    margin-left: 5px;
  }
}
</style>
