<script>
import Map from "./components/Map.vue";
import Form from "./components/Form.vue";
import * as turf from "@turf/turf";

export default {
  name: "App",
  components: {
    Map,
    Form,
  },
  data() {
    return {
      map: null,
      layer: null,
      style: {
        color: "#D63057",
        opacity: 0.5,
      },
    };
  },
  mounted() {
    this.map = this.$refs.map.map;
    this.addGeometry();
  },
  methods: {
    addGeometry() {
      const points = turf.randomPoint(3);
      const polygon = turf.convex(points);
      const line = turf.lineString(
        points.features.map((f) => f.geometry.coordinates)
      );
      const geometry = turf.featureCollection([
        polygon,
        line,
        ...points.features,
      ]);

      this.layer = {
        id: "geometry",
        type: "fill",
        source: {
          type: "geojson",
          data: geometry,
        },
        paint: {
          "fill-color": this.style.color,
          "fill-opacity": this.style.opacity,
        },
      };

      this.map.on("load", () => {
        this.map.addLayer(this.layer);
      });
    },

    changeStyle(style) {
      this.style = style;
      this.map.setPaintProperty(this.layer.id, "fill-color", this.style.color);
      this.map.setPaintProperty(
        this.layer.id,
        "fill-opacity",
        Number(this.style.opacity)
      );
    },

    exportGeoJSON() {
      const data = this.map.getSource(this.layer.id).serialize().data;
      const blob = new Blob([JSON.stringify(data)], {
        type: "application/json",
      });
      const url = URL.createObjectURL(blob);
      const link = document.createElement("a");

      link.href = url;
      link.download = `${this.layer.id}.geojson`;
      link.click();
      URL.revokeObjectURL(url);
    },
  },
};
</script>

<template>
  <div id="app">
    <h1 class="text-center text-xl">
      Тестове завдання на позицію Front End розробника
    </h1>
    <div class="flex">
      <div class="w-3/4">
        <Map ref="map" />
      </div>
      <div class="w-1/4 p-4">
        <Form @change-style="changeStyle" />
        <button
          class="p-2 border border-gray-600 mt-5 cursor-pointer hover:bg-slate-100 hover:text-black transition-colors duration-1000 ease-in-out"
          @click="exportGeoJSON"
        >
          Експортувати геометрію
        </button>
      </div>
    </div>
  </div>
</template>

<style>
#app {
  font-family: Arial, sans-serif;
  max-width: 1200px;
  margin: 0 auto;
}
</style>
