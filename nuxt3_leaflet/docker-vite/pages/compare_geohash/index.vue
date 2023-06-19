<script setup lang="ts">
import "leaflet/dist/leaflet.css";
import { LMap, LTileLayer, LMarker } from "@vue-leaflet/vue-leaflet";

const zoom1 = ref(20)
const zoom2 = ref(20)
const coordinates = [50, 50]

</script>

<template>
  <div id="map1" style="height:50%; width:100%">
    <l-map :use-global-leaflet="false" ref="map" v-model:zoom="zoom1" :center="[26.2123, 127.6791]">
      <l-tile-layer url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png" layer-type="base"
        name="OpenStreetMap"></l-tile-layer>
    </l-map>
  </div>
  <div id="map2" style="height:50%; width:100%">
    <l-map :use-global-leaflet="false" ref="map" v-model:zoom="zoom2" :center="[45.4156, 141.673]">
      <l-tile-layer url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png" layer-type="base"
        name="OpenStreetMap"></l-tile-layer>
    </l-map>
  </div>
</template>
<style>
#map2 {
  border-top: 2px solid grey;
}
</style>
