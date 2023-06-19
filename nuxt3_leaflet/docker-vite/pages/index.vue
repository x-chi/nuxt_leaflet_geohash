<script setup lang="ts">
const items = [
  {
    title: "Leaflet",
    to: "map",
  },
  {
    title: "geohash比較する君",
    to: "compare_geohash",
  },
  {
    title: "geohash比較する君2",
    to: "compare_geohash2",
  },
  {
    title: "Leaflet Marker Cluster",
    to: "map-cluster",
  },
  {
    title: "Text Selection with VueUse",
    to: "textSelection",
  },
];
</script>
<template>
  <v-container>
    <ul>
      <li class="mb-2" v-for="(item, key) in items" :key="key">
        <NuxtLink :to="item.to">
          {{ item.title }}
        </NuxtLink>
      </li>
    </ul>
  </v-container>
</template>
