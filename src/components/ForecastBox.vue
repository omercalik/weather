<template>
  <div class="forecast-box">
    <p>{{ date }}</p>
    <img
      class="forecast-image"
      :src="'/img/' + getImageSource() + '.png'"
      alt=""
    />
    <div class="degree-wrapper">
      <p class="degree-text">
        {{ degreeMax + "℃" }} {{ "  -  " + degreeMin + "℃" }}
      </p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue"

export default defineComponent({
  name: "ForeCastBox",
  props: ["date", "icon", "degreeMin", "degreeMax"],

  data() {
    return {}
  },

  mounted() {},

  methods: {
    getImageSource() {
      return this.icon.replace(/\s/g, "")
    },
  },
})
</script>

<style lang="scss">
.forecast-box {
  background: #1e213a;
  text-align: center;
  height: 200px;
  position: relative;

  img {
    width: 40%;
  }

  p {
    color: #e7e7eb;
    font-weight: 500;
    font-size: 16px;
  }

  .degree-wrapper {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  @media screen and (max-width: 1600px) {
    p {
      font-size: 13px;
    }
  }

  @media screen and (max-width: 900px) {
    width: 40%;
  }
}
</style>
