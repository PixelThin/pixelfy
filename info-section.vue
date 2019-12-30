<template>
  <v-row align="center" class="info-section d-flex">
    <div
      class="d-md-none section-image background-image"
      :style="{ backgroundImage: `url(${image})`}"
    ></div>
    <v-col class="text-center" cols="12" md="8" :order="textOrder">
      <slot name="title">
        <div class="text-uppercase font-weight-light display-2 mb-6">{{ title }}</div>
      </slot>
      <div class="mx-12 text-left">
        <slot></slot>
      </div>
    </v-col>
    <v-col
      class="section-image-col text-center background-image align-self-stretch d-none d-md-flex"
      :style="{ backgroundImage: `url(${image})`}"
      cols="12"
      md="4"
      v-scroll-reveal="fadeIn"
    ></v-col>
  </v-row>
</template>
<style lang="scss" scoped>
@import "~vuetify/src/styles/styles.sass";

.section-image {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  opacity: 0.15;
}

.section-image-col {
  min-height: 400px;
}

.info-section {
  min-height: 50vh;
  position: relative;
}

// TODO: Add icon support back in or remove the following:
.big-icon {
  @media screen and (max-width: map-get($grid-breakpoints,"sm")) {
    font-size: 14rem;
  }
  @media screen and (min-width: map-get($grid-breakpoints,"sm")) {
    font-size: 30rem;
  }
}
</style>
<script lang="ts">
import { fadeIn } from "@/lib/transitions";
import Vue from "vue";
export default Vue.extend({
  data: () => ({
    fadeIn
  }),
  computed: {
    textOrder() {
      if (typeof this.right === "undefined") {
        return "first";
      } else {
        return "last";
      }
    }
  },
  props: ["title", "icon", "right", "image"]
});
</script>
