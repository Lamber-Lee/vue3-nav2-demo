<template>
  <div>
    <Home></Home>
    <Weather clas="weather"></Weather>
    <Date class="date"></Date>
  </div>
</template>

<script setup>
import Home from './view/Home.vue';
import Weather from '@/components/Weather.vue';
import {startSakura} from '@/fullScreenFlower';
import Date from './components/Date.vue';


</script>


<style scoped>

</style>
