<script setup lang="ts">
import Test from '../../components/map_compare2/Test.vue';
import Test2 from '../../components/map_compare2/Test2.vue';
import Test3 from '../../components/map_compare2/Test3.vue';

import { useCounterStore } from "@/store/counter";
import { storeToRefs } from "pinia";
const counterStore = useCounterStore();
const { increment } = counterStore;
const { count } = storeToRefs(counterStore);

console.log("gbgthuyhjdsfdf")
</script>

<style>
#map1,
#map2,
#map3 {
  height: 500px;
  width: 800px;
}

#map1,
#map3 {
  border-top: 5px solid DimGray;
}
</style>


<template>
  <!-- <div>
    <h1>Count:{{ count }}</h1>
    <button @click="increment">たすよ</button>
  </div> -->

  <div id="map2">
    <ClientOnly>
      <!-- 宗谷岬 -->
      <Test2 />
    </ClientOnly>
  </div>
  <div id="map1">
    <ClientOnly>
      <!-- 沖縄美ら海水族館 -->
      <Test />
    </ClientOnly>
  </div>
  <div id="map3">
    <ClientOnly>
      <!-- インドネシア_グランドハイアット-->
      <Test3 />
    </ClientOnly>
  </div>
</template>
