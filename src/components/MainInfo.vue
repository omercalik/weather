<template>
  <div class="wrapper">
    <div v-if="!isSearching && weatherData" class="visual-info">
      <button @click="toggleSearch" class="toggle-search-btn">
        Search for places
      </button>
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
        Today •
        {{
          new Date(currentWeatherInfo.created).toLocaleString("en-GB", {
            day: "numeric",
            weekday: "long",
            month: "long",
          })
        }}
      </p>

      <img width="15" src="../assets/location.svg" />
      <span class="city-name">{{ weatherData.title }}</span>
    </div>

    <!-- Hidden Search Menu -->
    <div v-if="isSearching" class="search">
      <img
        @click="toggleSearch"
        class="close-icon"
        width="18"
        src="../assets/close.svg"
        alt=""
      />

      <div class="search-container">
        <div class="input-wrapper">
          <div class="input-container">
            <img src="../assets/loupe.svg" width="15" alt="" />
            <input
              placeholder="search location"
              class="search-input"
              type="text"
              onfocus="this.placeholder = ''"
              onblur="this.placeholder = 'search location'"
              v-model="location"
            />
          </div>
          <button @click="searchLocation" class="search-btn">Search</button>
        </div>

        <div class="search-result-container">
          <div
            v-for="location in locationResults"
            :key="location.woeid"
            class="search-result"
            @click="
              () => {
                getWeatherData(location.woeid)
              }
            "
          >
            {{ location.title }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import axios from "axios"
const auth = {
  headers: { "Accept": "application/json",
    "Access-Control-Allow-Origin": "*",
    "Accept-Version": "1", },
}
import { defineComponent } from "vue"

export default defineComponent({
  name: "MainInfo",

  props: ["weatherData"],
  data() {
    return {
      weatherImageSrc: "",
      currentWeatherInfo: {},
      isSearching: false,
      location: "",
      locationResults: [],
    }
  },

  mounted() {
    this.weatherImageSrc = this.weatherData.consolidated_weather[0].weather_state_name.replace(
      /\s/g,
      ""
    )
    this.currentWeatherInfo = this.weatherData.consolidated_weather[0]
  },

  watch: {
    location: function(newValue, oldValue) {
      console.log("old: ", oldValue)
      console.log("new", newValue)
    },
  },

  methods: {
    toggleSearch() {
      this.isSearching = !this.isSearching
    },

    async searchLocation() {
      if (this.location !== "") {
        let response = await axios.get(
          `${process.env.VUE_APP_API_BASE_URL}/location/search/?query=${this.location}`,
          auth
        )

        this.locationResults = response.data
      }
    },

    async getWeatherData(woeid: number) {
      console.log(woeid)
      let weather = await axios.get(
        `${process.env.VUE_APP_API_BASE_URL}/location/${woeid}`,
        auth
      )

      this.currentWeatherInfo = weather.data.consolidated_weather[0]

      this.$emit("changelocation", weather.data)
      this.isSearching = !this.isSearching
    },
  },
})
</script>

<style lang="scss">
.input-wrapper {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}
.search {
  position: relative;
  height: 100vh;
  width: 25vw;
  padding-left: 50px;
  background-color: #1e213a;
  z-index: 2;
  overflow: auto;

  @media screen and (max-width: 900px) {
    width: 100vw;
  }

  .search-result {
    color: #e7e7eb;
    font-weight: 500;
    font-size: 16px;
    margin-top: 72px;
    cursor: pointer;
    padding-top: 20px;
    padding-left: 10px;
    padding-bottom: 20px;
    margin-right: 20px;
    width: 85%;

    &:hover {
      border: 1px solid #616475;
    }
  }

  .close-icon {
    position: absolute;
    right: 50px;
    top: 20px;
    cursor: pointer;
  }

  .search-btn {
    background: #3c47e9;
    color: #e7e7eb;
    padding: 15px 20px;
    font-weight: 600;
    font-size: 16px;
    border: none;
    margin-right: 20px;
    cursor: pointer;

    &:hover {
      transition: all 0.3s;
      opacity: 0.9;
    }
  }

  .search-container {
    padding-top: 80px;

    .input-container {
      border: 1px solid #e7e7eb;
      width: 60%;
      min-width: 170px;
      display: flex;
      padding-left: 15px;
      flex-wrap: 1;
    }
  }

  .search-input {
    margin-right: 12px;
    background: transparent;
    border: none;
    outline: none;
    padding-top: 15px;
    padding-bottom: 15px;
    padding-left: 20px;
    color: #e7e7eb;

    &::placeholder {
      color: #616475;
      font-weight: 500;
      font-size: 16px;
    }
  }
}
.visual-info {
  height: 100%;

  width: 25vw;
  background-color: #1e213a;
  overflow: auto;

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
  .weather-image {
    margin-top: 50px;
  }

  .toggle-search-btn {
    display: block;
    background: #6e707a;
    font-weight: 500;
    font-size: 16px;
    color: #e7e7eb;
    padding: 10px 18px;
    margin-left: 60px;
    position: absolute;
    top: 15px;
    cursor: pointer;

    @media screen and (max-width: 1200px) {
      padding: 6px 12px;
      font-size: 12px;
    }
  }

  @media screen and (max-width: 1000px) {
    .weather-image {
      width: 60%;
    }
    .temperature {
      font-size: 70px;

      span {
        font-size: 25px;
        padding-left: 10px;
      }
    }
  }

  @media screen and (max-width: 900px) {
    width: 100vw;
    padding-bottom: 100px;

    .weather-image {
      width: 150px;
    }

    .temperature {
      font-size: 144px;
    }
  }
}
</style>
