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

  mounted() {
    console.log(this.date)
  },

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
  height: 250px;

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
}
</style>
