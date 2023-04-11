<script setup>
import { ref, reactive, watch, onMounted } from 'vue';
import * as echarts from 'echarts';

const props = defineProps(['data']);

const chartRef = ref(null);
const date = ref('');
const valeur = ref('');

/*const data = reactive([
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
    { x: '11/01/2022', y: 20 }
]);*/

const data = reactive(props.data);

onMounted(() => {
    const chart = echarts.init(chartRef.value);
    const option = {
        xAxis: {
            type: 'category',
            data: data.map(item => item.x),
            name: "Date (jour)"
        },
        yAxis: {
            type: 'value',
            name: 'Valeur de température (°C)'
        },
        series: [{
            data: data.map(item => item.y),
            type: 'line',
            color: 'navy'
        }]
    };
    chart.setOption(option);

    watch(data, () => {
        const updatedOptions = {
            xAxis: {
                data: data.map(item => item.x)
            },
            series: [{
                data: data.map(item => item.y)
            }]
        };
        chart.setOption(updatedOptions);
    });

});

function ajoutData() {
    if (date.value.length == 0 || valeur.value.length == 0) {
        alert("Veuillez saisir une date et une valeur de température");
    } else {
        const dateParts = date.value.split('-');
        const dateFormatte = `${dateParts[2]}/${dateParts[1]}/${dateParts[0]}`;
        data.push({ x: dateFormatte /*date.value*/, y: valeur.value });
        date.value = '';
        valeur.value = '';
    }
}
</script>


<template>
    <label for="input1">Selectionner une date :</label>
    <input id="input1" type="date" v-model="date" /><br>
    <label for="input2">Selectionner une valeur de température :</label>
    <input id="input2" type="number" min="0" max="60" v-model="valeur" /><br>
    <button class="ajout" @click="ajoutData">Ajouter</button>

    <div class="graphique" ref="chartRef"></div>
</template>


<style scoped>
#input1 {
    margin-left: 10px;
    margin-bottom: 1rem;
    border-radius: 5px;
    width: 10%;
    height: 18px;
}

#input2 {
    margin-left: 10px;
    margin-bottom: 1rem;
    border-radius: 5px;
    width: 17%;
    height: 18px;
}

.ajout {
    margin-bottom: 1rem;
    border-radius: 3px;
    cursor: pointer;
}

#title {
    text-align: center;
}

.graphique {
    width: 100%;
    height: 500px;
}
</style>
