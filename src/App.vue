<template>
  <div>
  <Home></Home>
  <Weather clas="weather"></Weather>

  </div>
</template>

<script setup>
import Home from "./view/Home.vue";
import Weather from "@/components/Weather.vue";
import {startSakura} from "@/fullScreenFlower";


</script>


<style scoped>

</style>
