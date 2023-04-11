<script setup>
import { ref } from 'vue';
import Graphe from './components/Graphe.vue';

const datasets = ref([
    { x: '01/01/2022', y: 50 },
    { x: '02/01/2022', y: 20 },
    { x: '03/01/2022', y: 40 },
    { x: '04/01/2022', y: 10 },
    { x: '05/01/2022', y: 30 },
    { x: '06/01/2022', y: 80 },
    { x: '07/01/2022', y: 70 },
    { x: '08/01/2022', y: 60 },
    { x: '09/01/2022', y: 90 },
    { x: '10/01/2022', y: 40 },
    { x: '11/01/2022', y: 20 },
    { x: '12/01/2022', y: 23 }
]);
</script>


<template>
  <h1 id="title">Test de Graphique avec Echart.js</h1><br>
  <Graphe :data="datasets"></Graphe>
</template>


<style scoped>
#title {
  text-align: center;
}
</style>
