<template>
  <l-map ref="map" :max-zoom="19" v-model:zoom="zoom" v-model:center="center" :zoomAnimation="true"
    :markerZoomAnimation="true" :useGlobalLeaflet="true" :options="{ zoomControl: false }" @ready="onLeafletReady">
    <template v-if="leafletReady">
      <l-control-layers position="bottomright" />
      <l-control-zoom position="bottomright" />
      <l-tile-layer v-for="tileProvider in tileProviders" :key="tileProvider.name" :name="tileProvider.name"
        :visible="tileProvider.visible" :url="tileProvider.url" :attribution="tileProvider.attribution"
        layer-type="base" />
    </template>
  </l-map>
</template>
<script>
import * as L from "leaflet";
import "leaflet.markercluster/dist/leaflet.markercluster.js";
import "leaflet/dist/leaflet.css";
import "leaflet.markercluster/dist/MarkerCluster.css";
import "leaflet.markercluster/dist/MarkerCluster.Default.css";
import {
  LMap,
  LTileLayer,
  LControlZoom,
  LControlLayers,
} from "@vue-leaflet/vue-leaflet";
import 'leaflet.gridline';

// data() {
//   return {
//     zoom: 3,
//     center: [54, 28],
//     tileProviders: [
//       {
//         name: "OpenStreetMap",
//         visible: true,
//         attribution:
//           '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
//         url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
//       },
//     ],

//     leafletReady: false,
//     leafletObject: null,
//   };
// },


export default {
  mounted() {
    L.Map.addInitHook(function () {

      const map = L.map('map').setView([51.505, -0.09], 13);

      // add an OpenStreetMap tile layer
      L.tileLayer('http://{s}.tile.osm.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
      }).addTo(map);

      console.log("cmdpcmdpvmdvfmpmp");

      // add to map
      const gridlineLayer = L.gridlineLayer().addTo(map);
    });
  };

  methods: {
    async onLeafletReady(map) {
      await this.$nextTick();
      this.leafletReady = true;
    },
  },
};
</script>
