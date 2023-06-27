<template>
  <!-- Componente que muestra información sobre la lista de tareas -->
  <div class="container-right">
    <h2>Información de tu lista de tareas</h2>
    <p>Tienes pendientes {{ contadordeTareas.pendientes }} {{ contadordeTareas.pendientes === 1 ? 'tarea' : 'tareas' }}</p>
    <p>Has completado {{ contadordeTareas.completadas }} {{ contadordeTareas.completadas === 1 ? 'tarea' : 'tareas' }}</p>
  </div>
</template>

<script>
export default {
  props: {
    // Propiedad que recibe el array de tareas
    tareas: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      // Estado del componente
      contadordeTareas: {
        pendientes: 0,
        completadas: 0
      }
    };
  },
  computed: {
    contadorTareas() {
      // Computed property que calcula el número de tareas pendientes y completadas
      const pendientes = this.tareas.filter(tarea => tarea.estado === 'Pendiente').length;
      const completadas = this.tareas.filter(tarea => tarea.estado === 'Completada').length;
      return {
        pendientes,
        completadas
      };
    }
  },
  watch: {
    tareas: {
      // Watcher que se activa cuando la propiedad 'tareas' cambia
      handler() {
        // Llama al método para actualizar el contador de tareas
        this.actualizarContador();
      },
      deep: true
    }
  },
  methods: {
    actualizarContador() {
      // Método que actualiza el contador de tareas pendientes y completadas
      const { pendientes, completadas } = this.contadorTareas;
      this.contadordeTareas.pendientes = pendientes;
      this.contadordeTareas.completadas = completadas;
    }
  },
  created() {
    // Hook created que se ejecuta al crear el componente
    // Llama al método para actualizar el contador inicialmente
    this.actualizarContador();
  }
};
</script>

  
<style scoped>
/* Estilos para el container-right y sus elementos */
.container-right {
  font-family: 'Noto Sans', sans-serif;
  width: 30%;
  background-color: #f2f2f2;
  border-radius: 10px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  padding: 20px;
  height: 160px;
  margin-top: 20px;
  align-items: center;
  text-align: center;
  margin-right: 85px;
  animation: floatAnimation 4s infinite;
}
/* Animacion para hacer parecer que el elemento esta flotando */
@keyframes floatAnimation {
  0%, 100% {
    transform: translateY(-20px);
  }
  50% {
    transform: translateY(20px);
  }
}

.container-right h2 {
  font-size: 24px;
  margin-bottom: 10px;
}

.container-right p {
  font-size: 16px;
  margin-bottom: 8px;
}

/* Estilos para dispositivos móviles */
@media (max-width: 767px) {
  .container-right{
    padding: 30px;
    margin-left: auto;
    margin-right: auto;
    width: 60%;
    margin-top: 140px;
  }
}
/* Estilos para tablets */
@media (min-width: 768px) and (max-width: 1250px) {
  .container-right{
    margin-left: auto;
    margin-right: auto;
    width: 60%;
    margin-top: 130px;
    padding: 15px;
  }
}
</style>

  