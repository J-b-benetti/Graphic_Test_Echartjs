<script setup>
import { ref, reactive, watch, onMounted } from 'vue';
import * as echarts from 'echarts';

const props = defineProps(['data']);    // On définit les entrées du composant

const chartRef = ref(null);
const date = ref('');   // Fait référence au v-model
const valeur = ref(''); // Fait référence au v-model

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
            name: "Date (jour)",
            axisLine: {
                lineStyle: {
                    color: 'black',
                    width: 1,
                    type: 'solid'
                }
            },
            nameTextStyle: {
                fontWeight: 'bolder',
                fontSize: 15
            }
        },
        yAxis: {
            type: 'value',
            name: 'Valeur de température (°C)',
            axisLine: {
                lineStyle: {
                    color: 'black',
                    width: 1,
                    type: 'solid'
                }
            },
            nameTextStyle: {
                fontWeight: 'bolder',
                fontSize: 15
            }
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
    <label class="label_input" for="input1">Selectionner une date :</label>
    <input id="input1" type="date" v-model="date" /><br>
    <label class="label_input" for="input2">Selectionner une valeur de température :</label>
    <input id="input2" type="number" min="0" max="60" v-model="valeur" /><br>
    <button class="ajout_button" @click="ajoutData">Ajouter</button>

    <div class="graphique" ref="chartRef"></div>
</template>


<style scoped>
.label_input {
    font-size: 20px;
    margin-left: 1.5rem;
}

#input1 {
    margin-left: 10px;
    margin-bottom: 1rem;
    border-radius: 5px;
    width: 10%;
    height: 20px;
}

#input2 {
    margin-left: 10px;
    margin-bottom: 1rem;
    border-radius: 5px;
    width: 17%;
    height: 18px;
}

.ajout_button {
    margin-bottom: 1rem;
    margin-left: 1.5rem;
    border-radius: 3px;
    font-size: 20px;
    cursor: pointer;
    color: black;
    font-weight: bold;
    background-color: aquamarine;
}

.graphique {
    width: 100%;
    height: 500px;
}
</style>
