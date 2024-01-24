<script>
import maplibre from "maplibre-gl";

export default {
  name: "Map",
  props: {
    center: {
      type: Array,
      default: () => [30.5234, 50.4501],
    },
    zoom: {
      type: Number,
      default: 5,
    },
  },
  data() {
    return {
      map: null,
    };
  },
  mounted() {
    this.map = new maplibre.Map({
      container: this.$refs.map,
      style: "https://demotiles.maplibre.org/style.json",
      center: this.center,
      zoom: this.zoom,
    });
  },
};
</script>

<template>
  <div class="h-[600px]" ref="map"></div>
</template>
