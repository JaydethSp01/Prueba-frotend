<template>
  <!-- Capa de superposición del formulario -->
  <div class="overlay" :style="{ visibility: visibility ? 'visible' : 'hidden' }">
    <!-- Contenedor del formulario -->
    <div class="popup">
      <!-- Botón de cierre del formulario -->
      <i class="fa-solid fa-circle-xmark btn-cerrar" @click="cerrarFormulario"></i>
      <!-- Formulario -->
      <form class="task-form" @submit.prevent="agregarTarea">
        <h2>Agregar Nueva Tarea</h2>
        <!-- Campo de texto para el título de la tarea -->
        <input type="text" class="task-input" v-model="nuevaTarea" placeholder="Título de la tarea" pattern="[A-Za-z\s]*[0-9]?[A-Za-z\s]*" />
        <!-- Botón para guardar la tarea -->
        <button type="submit" class="task-button">Guardar tarea</button>
      </form>
    </div>
  </div>
</template>

<script>
//importando dependecias necesarias
import { ref, inject } from 'vue';
import swal from 'sweetalert';

export default {
  props: {
    agregar_Tarea: {
      type: Function,
      required: true
    },
    visibility: {
      type: Boolean,
      default: false
    }
  },
  setup() {
    // Variable reactiva para almacenar el valor del campo de texto
    const nuevaTarea = ref('');
    // Inyectar la instancia del componente padre (taskList)
    const taskList = inject('taskList');
    /**
     * Agrega una nueva tarea a la lista de tareas
     */
    const agregarTarea = () => {
      // Verificar si el campo de texto no está vacío
      if (nuevaTarea.value.trim() !== '') {
        // Crear un objeto de tarea con el nombre y estado
        const tarea = { nombre: nuevaTarea.value, estado: 'Pendiente' };
        // Verificar si la tarea ya está registrada (comparación sin distinguir mayúsculas y minúsculas)
        if (taskList.tareas.some(t => t.nombre.toLowerCase() === nuevaTarea.value.toLowerCase())) {
          // Mostrar una notificación de error si la tarea está duplicada
          swal('Oops', 'La tarea ya está registrada', 'error');
        } else {
          // Agregar la nueva tarea a la lista de tareas
          taskList.agregarTarea(tarea);
          // Limpiar el campo de texto
          nuevaTarea.value = '';
          // Cerrar el formulario
          cerrarFormulario();
        }
      } else {
        // Mostrar una notificación de error si el campo de texto está vacío
        swal('Oops', 'Ingrese un título para la tarea', 'error');
      }
    };
    /**
     * Cierra el formulario
     */
    const cerrarFormulario = () => {
      taskList.cerrarFormulario();
    };

    // Retornar las variables y funciones para ser utilizadas en el template
    return {
      nuevaTarea,
      agregarTarea,
      cerrarFormulario
    };
  }
};
</script>

    
<style scoped>
/* Estilos del overlay y el popup */
.overlay {
  background-color: rgba(0, 0, 0, 0.5);
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  visibility: hidden;
}

.popup {
  background-color: #f2f2f2;
  box-shadow: 0px 0px 5px 0px rgba(0, 0, 0, 0.4);
  font-family: 'Noto Sans', sans-serif;
  text-align: center;
  padding: 20px;
  width: 30%;
  border-radius: 10px;
}

.popup .btn-cerrar {
  font-size: 25px;
  display: block;
  text-align: right;
  line-height: 16px;
  cursor: pointer;
}

.popup .btn-cerrar:hover {
  color: rgb(172, 23, 23);
}

/* Estilos del formulario de tareas */
.task-form {
  margin-top: 20px;
}

h2 {
  margin-bottom: 40px;
}

.task-input {
  padding: 10px;
  margin-bottom: 30px;
  border: 1px solid #00c0ff;
  border-radius: 4px;
  font-size: 16px;
  outline: none;
  border-radius: 10px;
  width: 80%;
}

.task-input:focus {
  border-color: #00c0ff;
}

.task-button {
  padding: 8px 16px;
  background-color: #00425b;
  color: white;
  font-size: 14px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
}

.task-button:hover {
  background-color: #00749f;
}

/* Estilos para dispositivos móviles */
@media (max-width: 767px) {
  .popup {
    width: 60%;
    padding: 10px 10px 18px 10px;
  }
  .popup .btn-cerrar {
    font-size: 20px;
  }
}
</style>
