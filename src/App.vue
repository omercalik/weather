<template>
  <div class="main-wrapper" v-if="Object.keys(weatherData).length > 0">
    <MainInfo :weatherData="weatherData" />
    <DetailedInfo :weatherData="weatherData" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue"
import MainInfo from "./components/MainInfo.vue"
import DetailedInfo from "./components/DetailedInfo.vue"

import axios from "axios"

export default defineComponent({
  name: "App",
  components: { MainInfo, DetailedInfo },

  data() {
    return {
      weatherData: {},
    }
  },

  mounted() {
    this.fetchWeatherData()
  },

  methods: {
    async fetchWeatherData() {
      let id = await axios.get(
        `${process.env.VUE_APP_API_BASE_URL}/location/search/?query=ankara`
      )

      let weather = await axios.get(
        `${process.env.VUE_APP_API_BASE_URL}/location/${id.data[0].woeid}`
      )

      this.weatherData = weather.data
    },
  },
})
</script>

<style lang="scss">
#app {
  font-family: "Raleway", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

*,
*::after,
*::before {
  box-sizing: border-box;
}

body {
  margin: 0;
}

.main-wrapper {
  display: flex;
}
</style>
