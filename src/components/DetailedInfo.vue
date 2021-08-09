<template>
  <div v-if="weatherData" class="detailed-info-wrapper">
    <div class="container">
      <div class="forecast-wrapper">
        <ForeCastBox
          v-for="(forecast, index) in foreCast"
          :key="forecast.id"
          :date="getDate(index, forecast.applicable_date)"
          :icon="forecast.weather_state_name"
          :degreeMin="Math.round(forecast.min_temp)"
          :degreeMax="Math.round(forecast.max_temp)"
        />
      </div>
      <h2>Bugünün Detayları</h2>

      <div class="detail-wrapper">
        <div class="row">
          <DetailBox
            title="Rüzgar Hızı ve Yönü"
            type="wind"
            :data="{
              speed: Math.random(todaysData.wind_speed),
              direction: todaysData.wind_direction_compass,
            }"
          />
          <DetailBox
            title="Nem"
            type="humidity"
            :data="{ humidity: todaysData.humidity }"
          />
        </div>
        <div class="row">
          <DetailBox
            title="Görüş Mesafesi"
            type="visibility"
            :data="{ visibility: todaysData.visibility }"
          />
          <DetailBox
            title="Hava Basıncı"
            type="pressure"
            :data="{ pressure: Math.round(todaysData.air_pressure) }"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue"
import ForeCastBox from "./ForecastBox.vue"
import DetailBox from "./DetailBox.vue"

export default defineComponent({
  name: "DetailedInfo",

  components: {
    ForeCastBox,
    DetailBox,
  },

  props: ["weatherData"],

  data() {
    return {
      foreCast: [],
      todaysData: {},
    }
  },

  mounted() {
    this.foreCast = this.weatherData.consolidated_weather.slice(1)
    this.todaysData = this.weatherData.consolidated_weather[0]
    console.log(this.foreCast)
  },

  methods: {
    getDate(index: number, date: string) {
      return index === 0
        ? "Yarın"
        : new Date(date).toLocaleString("default", {
            day: "numeric",
            weekday: "long",
            month: "long",
          })
    },
  },
})
</script>

<style lang="scss">
.detailed-info-wrapper {
  width: 75vw;
  background: #100e1d;

  h2 {
    color: #e7e7eb;
    font-weight: 700;
    font-size: 24px;
    margin-top: 50px;
  }

  .container {
    width: max-content;
    margin: 0 auto;
  }
}

.forecast-wrapper {
  display: flex;
  width: max-content;
  gap: 25px;
  margin-top: 100px;
}

.detail-wrapper {
  .row {
    display: flex;
    justify-content: space-between;
    margin-bottom: 50px;
  }
}
</style>
