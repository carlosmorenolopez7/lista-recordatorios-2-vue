<script setup>
    import { ref } from 'vue'
    import { computed } from 'vue'
    const recordatorios = ref(JSON.parse(localStorage.getItem('recordatorios')) || []);
    const texto = ref("");
    
        function nuevoRecordatorio(){
            let long = recordatorios.value.length;
            let info = {
                id: long + 1,
                titulo: texto.value,
                hecho: false,
                prioridad: 2,
                fecha: Date.now()
            };
            recordatorios.value.push(info);
            texto.value = "";
            localStorage.setItem('recordatorios', JSON.stringify(recordatorios.value));
        }

        function borrarCompletados(){
            let arrayFilt = recordatorios.value.filter((entrada) => !entrada.hecho);
            recordatorios.value = arrayFilt;
            localStorage.setItem('recordatorios', JSON.stringify(recordatorios.value));
        }
        
        const entradasPendientes = computed(() =>{
            return recordatorios.value.filter((recordatorio) => recordatorio.hecho == false).length;
        });

        const totalEntradas = computed(() =>{
            return recordatorios.value.length;
        });
</script>

<template>
    <h1>Proyecto recordatorio.js - Carlos Moreno López</h1>
    <input type="text" id="reminder" placeholder="¿Qué quieres recordar?" v-model="texto" @keyup.enter="nuevoRecordatorio()"/>
    <hr>
        <p id="tareas"><i class="fa-solid fa-chart-column"></i> <span id="pendientes">{{entradasPendientes}} pendientes de un total de {{totalEntradas}}</span>&nbsp;|&nbsp;<span id="deleteAll"  @click="borrarCompletados"><i class="fa-solid fa-x"></i> Borrar tareas completadas</span></p>
    <hr/>
</template>

<style>
</style>