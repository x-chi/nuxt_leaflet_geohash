
<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue';
import * as L from "leaflet";
import "leaflet.markercluster/dist/leaflet.markercluster.js";
import "leaflet/dist/leaflet.css";
import "leaflet.markercluster/dist/MarkerCluster.css";
import "leaflet.markercluster/dist/MarkerCluster.Default.css";
import {
  LMap,
  LTileLayer,
  LControlZoom,
  LControlScale,
} from "@vue-leaflet/vue-leaflet";
import Geohash from "latlon-geohash";
import last from "lodash/last";

const zoom = ref(12);
const center = ref([-6.1918607858792525, 106.82252260060203]);
const tileProviders = ref([
  {
    name: 'OpenStreetMap',
    visible: true,
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
    url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
  },
]);


let leafletReady = ref(false);
let leafletObject = ref(null);

async function onLeafletReady(map) {
  await nextTick();
  leafletReady.value = true;
}

onMounted(() => {
  L.Map.addInitHook(function () {
    L.GridlineLayer = L.FeatureGroup.extend({
      initialize(options = {}) {
        // polyline options
        const defaultOptions = {
          color: '#979797',
          weight: '1',
          geohashLength: 5,
        };
        Object.assign(this.options, defaultOptions, options);
        this.mapHandles = [];
      }
      ,
      onAdd(map) {
        this._map = map;
        this.layer = L.featureGroup().addTo(this._map);
        this._draw();
        this._bindMapEvents();
        return this;
      },
      onRemove() {
        this._map.removeLayer(this.layer);
        this.mapHandles.forEach(({ type, handle }) => this._map.off(type, handle));
        this.mapHandles = [];
      },
      _draw() {
        this.layer.clearLayers();

        // get map bottom left latlng
        const bounds = this._map.getBounds();
        const mapSWPoint = bounds.getSouthWest();
        const mapNEPoint = bounds.getNorthEast();
        const SWGeohash = Geohash.encode(
          mapSWPoint.lat,
          mapSWPoint.lng,
          this.options.geohashLength
        );

        const geohashSWBounds = Geohash.bounds(SWGeohash);
        const latStep = Math.abs(geohashSWBounds.sw.lat - geohashSWBounds.ne.lat);
        const lngStep = Math.abs(geohashSWBounds.sw.lon - geohashSWBounds.ne.lon);

        // 网格线刻度值
        const latSplit = [geohashSWBounds.sw.lat];
        while (last(latSplit) < mapNEPoint.lat) {
          latSplit.push(last(latSplit) + latStep);
        }
        const lngSplit = [geohashSWBounds.sw.lon];
        while (last(lngSplit) < mapNEPoint.lng) {
          lngSplit.push(last(lngSplit) + lngStep);
        }

        // draw lat splits
        latSplit.forEach((lat) => {
          const polylineLayer = L.polyline(
            [
              [lat, lngSplit[0]],
              [lat, last(lngSplit)],
            ],
            this.options
          );
          this.layer.addLayer(polylineLayer);
        });
        // draw lng splits
        lngSplit.forEach((lng) => {
          const polylineLayer = L.polyline(
            [
              [latSplit[0], lng],
              [last(latSplit), lng],
            ],
            this.options
          );
          this.layer.addLayer(polylineLayer);
        });
      },
      _bindMapEvents() {
        const redraw = () => this._draw();
        ['moveend', 'zoomend'].forEach((event) => {
          this._map.on(event, redraw);
          this.mapHandles.push({ type: event, handle: redraw });
        });
      },
    });

    L.gridlineLayer = function (options) {
      return new L.GridlineLayer(options);
    };

    L.gridlineLayer().addTo(this);

    console.log("mdpcmdpmdpsvmfppvmfpmvpf")
  });

});

</script>
<template>
  <l-map ref="map" :max-zoom="19" v-model:zoom="zoom" v-model:center="center" :zoomAnimation="true"
    :markerZoomAnimation="true" :useGlobalLeaflet="true" :options="{ zoomControl: false }">
    <!-- <template v-if="leafletReady"> -->
    <l-control-zoom position="bottomright" />
    <l-control-scale position="topright" :imperial="false" :metric="true" />
    <l-tile-layer v-for="tileProvider in tileProviders" :key="tileProvider.name" :name="tileProvider.name"
      :visible="tileProvider.visible" :url="tileProvider.url" :attribution="tileProvider.attribution" layer-type="base" />
    <!-- </template> -->
  </l-map>
</template>
