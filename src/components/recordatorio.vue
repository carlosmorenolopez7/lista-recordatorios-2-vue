<script setup>
        import { ref } from 'vue'
        import { computed } from 'vue'
        const recordatorios = ref(JSON.parse(localStorage.getItem('recordatorios')) || []);
        const texto = ref("");
        
        function cambiarPrioridad(entrada, prioridad){
            entrada.prioridad = prioridad;
            localStorage.setItem('recordatorios', JSON.stringify(recordatorios.value))
        }

        function borrarRecordatorio(entrada){
            let index = recordatorios.value.indexOf(entrada);
            recordatorios.value.splice(index, 1);
            localStorage.setItem('recordatorios', JSON.stringify(recordatorios.value));
        }

        function marcarRecordatorio(entrada){
            entrada.hecho = !entrada.hecho;
            localStorage.setItem('recordatorios', JSON.stringify(recordatorios.value));
        }

        const entradasFiltradas = computed(() =>{
            let arrayFilt = recordatorios.value.filter((entrada) => entrada.titulo.toLowerCase().includes(texto.value.toLowerCase()));
            return arrayFilt.sort(function(a, b) {
                return b.prioridad - a.prioridad;
            });
        })
</script>

<template>
    <div id="recordatorios">
        <div class="singleReminder" v-for="recordatorio in entradasFiltradas" :key="recordatorio.id">
            <div class="reminderContent">
                <i v-if="recordatorio.hecho" class="fa-regular fa-check-circle" @click="marcarRecordatorio(recordatorio)"></i>
                <i v-else class="fa-regular fa-circle" @click="marcarRecordatorio(recordatorio)"></i>
                <h2 v-bind:class="{checked:recordatorio.hecho}">{{recordatorio.titulo}}</h2>
                <i class="fa-solid fa-square-minus" @click="borrarRecordatorio(recordatorio)"></i>
            </div>
            <div class="reminderOptions">
                <p>Prioridad</p>
                <button id="low" :class="[{marked:recordatorio.prioridad == 1}, {not_marked:recordatorio.prioridad != 1}]" @click="cambiarPrioridad(recordatorio, 1)"><i class="fa-solid fa-arrow-down"></i>Low</button>
                <button id="medium" :class="[{marked:recordatorio.prioridad == 2}, {not_marked:recordatorio.priordad != 2}]" @click="cambiarPrioridad(recordatorio, 2)">Normal</button>
                <button id="high" :class="[{marked:recordatorio.prioridad == 3}, {not_marked:recordatorio.priordad != 3}]" @click="cambiarPrioridad(recordatorio, 3)"><i class="fa-solid fa-arrow-up"></i>High</button>
                <i class='fa-regular fa-clock'></i>
                <p>Añadido hace {{Math.floor(((Date.now() - recordatorio.fecha)/1000)/60)}} minutos</p>
            </div>
        </div>
    </div>
</template>