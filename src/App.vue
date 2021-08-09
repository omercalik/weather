<template>
  <div class="main-wrapper" v-if="Object.keys(weatherData).length > 0">
    <MainInfo :weatherData="weatherData" @changelocation="changeLocation" />
    <DetailedInfo :weatherData="weatherData" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue"
import MainInfo from "./components/MainInfo.vue"
import DetailedInfo from "./components/DetailedInfo.vue"

import axios from "axios"
const auth = {
  headers: {
    "Accept": "application/json",
    "Access-Control-Allow-Origin": "*",
    "Access-Control-Allow-Methods": "GET,HEAD,OPTIONS,POST,PUT",
    "Access-Control-Allow-Headers": "Origin, X-Requested-With, Content-Type, Accept, Authorization, access-control-allow-headers, access-control-allow-methods",
    "Accept-Version": "1",
  },
}

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
      let location = await axios.get(
        `https://get.geojs.io/v1/ip/geo.json`,auth
        
      )

      let id = await axios.get(
        `https://cors-anywhere.herokuapp.com/${process.env.VUE_APP_API_BASE_URL}/location/search/?query=${location.data.city}`,
        auth
      )

      if (id.data.length > 0) {
        let weather = await axios.get(
          `https://cors-anywhere.herokuapp.com/${process.env.VUE_APP_API_BASE_URL}/location/${id.data[0].woeid}`,
          auth
        )

        console.log(weather)

        this.weatherData = weather.data
      } else {
        let weather = await axios.get(
          `https://cors-anywhere.herokuapp.com/${process.env.VUE_APP_API_BASE_URL}/location/2343732`,
          auth
        )

        this.weatherData = weather.data
     }
    },

    changeLocation(newData: any) {
      console.log(newData)
      this.weatherData = newData
    },

    getLocation(){

    }
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

::-webkit-scrollbar {
  display: none;
}

body {
  margin: 0;
}

.main-wrapper {
  display: flex;
  height: 100vh;

  @media screen and (max-width: 900px) {
    display: block;
  }
}
</style>
