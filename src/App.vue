<template>
  <!-- Encabezado -->
  <header>
    <nav>
      <h1 class="title">App List</h1>
    </nav>
  </header>

  <!-- Contenido principal -->
  <div class="Content">
    <!-- Componente TaskList -->
    <TaskList/>
  </div>
</template>

<script setup>
import TaskList from './components/TaskList.vue'
</script>

<style>
/* Importando fuentes necesarias de Google Fonts */
@import url('https://fonts.googleapis.com/css2?family=Lobster&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans&display=swap');

/* Estilos generales del body */
body {
  margin: 0;
  padding: 0;
}

/* Estilos del encabezado */
header {
  background-color: #021216fd;
  width: 100%;
  display: flex;
  justify-content: center;
}

/* Estilos del título */
h1 {
  font-family: 'Lobster', cursive;
  font-size: 40px;
  color: #ffff;
}
</style>
