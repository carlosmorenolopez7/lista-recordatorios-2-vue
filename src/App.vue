<script setup>
import cabecera from './components/header.vue'
import recordatorios from './components/recordatorio.vue'
import pie from './components/footer.vue'
</script>

<template>
<body>
    <header>
        <cabecera></cabecera>
    </header>
    <main>
        <recordatorios></recordatorios>
    </main>
    <footer>
        <pie></pie>
    </footer>
</body>
</template>

<style>
</style>