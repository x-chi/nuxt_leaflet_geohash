<template>
  <div class="map-test">
    <ClientOnly>
      <map-test />
    </ClientOnly>
  </div>

  <div>
    <h1>Count:{{ count }}</h1>
    <button @click="increment">たすよ</button>
  </div>
</template>

<script setup lang="ts">

import { useCounterStore } from "@/store/counter";
import { storeToRefs } from "pinia";
const counterStore = useCounterStore();
const { increment } = counterStore;
const { count } = storeToRefs(counterStore);

</script>



<style>
.map-test {
  height: 500px;
}
</style>
