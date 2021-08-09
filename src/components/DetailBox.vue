<template>
  <div class="detail-box">
    <h5 class="title">{{ title }}</h5>
    <div v-if="type === 'wind'" class="wind">
      <p>{{ (data.speed * 1.61).toPrecision(2) }} <span>kmh</span></p>
      <p class="direction">{{ data.direction }}</p>
    </div>
    <div v-else-if="type === 'humidity'" class="humidity">
      <p>{{ data.humidity }} <span>%</span></p>
      <div class="humidity-mag">
        <p>0</p>
        <p>50</p>
        <p>100</p>
      </div>
      <div id="humidity-bar">
        <div :style="{ width: `${data.humidity}%` }" class="progress"></div>
      </div>
    </div>
    <div v-else-if="type === 'visibility'" class="visibility">
      <p>{{ (data.visibility * 1.61).toPrecision(2) }} <span>km</span></p>
    </div>
    <div v-else class="pressure">
      <p>{{ data.pressure }} <span>mb</span></p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue"

export default defineComponent({
  name: "DetailBox",

  data() {
    return {}
  },

  props: ["title", "type", "data"],

})
</script>

<style lang="scss">
.detail-box {
  background: #1e213a;
  width: 480px;
  max-width: 45%;
  color: #e7e7eb;
  text-align: center;

  .title {
    font-weight: 500;
    font-size: 16px;
  }

  p {
    margin-top: -15px;
    font-weight: 700;
    font-size: 64px;

    @media screen and (max-width: 1600px) {
      font-size: 48px;
    }

    @media screen and (max-width: 1200px) {
      font-size: 40px;
    }
  }

  .direction {
    font-size: 14px;
    font-weight: 500;
    margin-top: -30px;
  }

  span {
    font-weight: 500;
    font-size: 36px;
  }

  @media screen and (max-width: 900px) {
    width: 100%;
    max-width: 100%;
    padding-top: 30px;
    padding-bottom: 30px;
    margin-bottom: 10px;
  }
}
.humidity-mag {
  display: flex;
  width: 80%;
  margin: 0 auto;
  margin-top: -30px;
  justify-content: space-between;

  p {
    font-size: 12px;
  }
}

#humidity-bar {
  width: 80%;
  background: #e7e7eb;
  border-radius: 80px;
  margin: 0 auto;
  margin-top: -8px;
  .progress {
    height: 10px;
    background-color: #ffec65;
    border-radius: 80px;
  }
}
</style>
