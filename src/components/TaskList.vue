<template>
 <div class="gradient">
     <!-- Contenedor principal de la lista de tareas -->
  <div class="task-list-container">
    <!-- Tabla que muestra las tareas -->
  <table ref="taskTable" class="task-table">
  <thead>
    <tr>
      <th>Tareas</th>
      <th>Estado</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <!-- Verificar si hay tareas, de lo contrario mostrar mensaje -->
    <template v-if="tareas.length > 0">
      <!-- Iterar sobre las tareas y mostrar cada una en una fila de la tabla -->
      <tr v-for="tarea in tareas" :key="tarea.id" :class="{ 'completada': tarea.completada }">
        <!-- Nombre de la tarea -->
        <td :class="{ 'task-realizada': tarea.completada }">{{ tarea.nombre }}</td>
        <!-- Estado de la tarea -->
        <td :class="{ 'task-realizada': tarea.completada }">{{ tarea.estado }}</td>
        <!-- Opciones de la tarea: Checkbox para marcar/completar y botón de eliminación -->
        <td class="options">
          <input type="checkbox" class="task-checkbox" :checked="tarea.completada" @change="marcarTarea(tarea)" />
          <i class="fas fa-trash-can delete-icon" @click="eliminarTarea(tarea.id)"></i>
        </td>
      </tr>
    </template>
    <template v-else>
      <tr>
        <td colspan="3" class="no-tasks">No hay tareas disponibles en este momento.</td>
      </tr>
    </template>
  </tbody>
</table>

    <!-- Botón para mostrar el formulario de agregar tarea -->
  <div class="boton-container">
    <button class="add-button" @click="mostrarFormulario"><i class="fas fa-plus"></i>Agregar Tarea</button>
  </div>
    <!-- Componente InfoTask para mostrar información adicional sobre las tareas -->
    <InfoTask :tareas="tareas" />
  </div>
  <!-- Componente AddTaskForm para agregar nuevas tareas -->
  <AddTaskForm @agregarTarea="agregarTarea" :visibility="formularioVisible" />
 </div>
</template>

<script>
  // Importar las dependencias necesarias
  import { v4 as uuidv4 } from 'uuid';
  import InfoTask from './InfoTask.vue';
  import AddTaskForm from './AddTaskForm.vue';
  import swal from 'sweetalert';

  export default {
    components: {
      InfoTask,
      AddTaskForm
    },
    data() {
      return {
        tareas: [],                 // Almacenar las tareas
        formularioVisible: false,    // Controlar la visibilidad del formulario
      };
    },
    provide() {
      return {
        taskList: this             // Proporcionar acceso a la lista de tareas a los componentes secundarios
      };
    },
    created() {
      this.cargarTareasGuardadas(); // Cargar las tareas almacenadas al iniciar el componente
    },
    methods: {
      cargarTareasGuardadas() {
        const tareasGuardadas = localStorage.getItem('tareas'); // Obtener las tareas almacenadas en el almacenamiento local
        console.log(tareasGuardadas);
        if (tareasGuardadas) {
          this.tareas = JSON.parse(tareasGuardadas); // Si hay tareas almacenadas, cargarlas en el arreglo de tareas
          // Asignar nuevos UUID a las tareas existentes
          this.tareas.forEach(tarea => {
            tarea.id = uuidv4(); // Asignar un nuevo UUID a cada tarea existente
          });
        } else {
          // Si no hay tareas almacenadas, crear tareas de ejemplo
          this.tareas = [
            { id: uuidv4(), nombre: 'Tarea 1', estado: 'Pendiente', completada: false },
            { id: uuidv4(), nombre: 'Tarea 2', estado: 'Pendiente', completada: false },
            { id: uuidv4(), nombre: 'Tarea 3', estado: 'Pendiente', completada: false },
            { id: uuidv4(), nombre: 'Tarea 4', estado: 'Pendiente', completada: false }
          ];
          this.guardarTareas(); // Guardar las tareas creadas en el almacenamiento local
        }
      },
      guardarTareas() {
        localStorage.setItem('tareas', JSON.stringify(this.tareas)); // Almacenar las tareas en el almacenamiento local
      },
      agregarTarea(tarea) {
        const nuevaTarea = {
          id: uuidv4(),              // Generar un nuevo UUID para la tarea
          nombre: tarea.nombre,
          estado: tarea.estado,
          completada: false
        };
        this.tareas.push(nuevaTarea); // Agregar la nueva tarea al arreglo de tareas
        this.guardarTareas();        // Guardar las tareas actualizadas en el almacenamiento local
      },
      eliminarTarea(id) {
        this.tareas = this.tareas.filter((tarea) => tarea.id !== id); // Eliminar la tarea con el ID proporcionado
        this.guardarTareas();                                      // Guardar las tareas actualizadas en el almacenamiento local
      },
      mostrarFormulario() {
        this.formularioVisible = true; // Mostrar el formulario para agregar una nueva tarea
      },
      cerrarFormulario() {
        this.formularioVisible = false; // Cerrar el formulario para agregar una nueva tarea, pasandolo a su valor inicial que es false
      },
      marcarTarea(tarea) {
        tarea.completada = !tarea.completada;                  // Cambiar el estado de completada de la tarea
        tarea.estado = tarea.completada ? 'Completada' : 'Pendiente'; // Actualizar el estado de la tarea
        this.guardarTareas();                                 // Guardar las tareas actualizadas en el almacenamiento local
        this.verificarTareasCompletadas();                    // Verificar si todas las tareas están completadas
      },
      verificarTareasCompletadas() {
        const todasCompletadas = this.tareas.every(tarea => tarea.completada); // Verificar si todas las tareas están completadas
        if (todasCompletadas) {
          this.mostrarNotificacion(); // Mostrar una notificación si todas las tareas están completadas
        }
      },
      mostrarNotificacion() {
        swal({
          title: '¡Buen trabajo!',              
          text: '¡Has completado todas las tareas!', 
          icon: 'success',                      
          button: '¡Genial!'                     
        });
      },
      
    }
  };
</script>




<style scoped>
/* En este style scoped que limita los estilos solo a este componente se agrega los estilos correspondiente a la TaskList */

/* Estilos para los elementos con la clase 'task-realizada', que representan las tareas completadas */
.task-realizada {
  color: gray;                  /* Cambia el color del texto a gris */
  text-decoration: line-through; /* Agrega una línea en el texto para indicar que está completado */
}

/* Estilos para el elemento con la clase 'gradient', que representa un fondo gradiente */
.gradient {
  padding-top: 5px;              /* Espacio superior de 5px */
  width: 100%;                   /* Ancho del 100% */
  height: 738px;                 /* Altura de 668px */
   background: linear-gradient(-45deg, #003555, #00a6ea, #ffffff); /*Fondo con un gradiente lineal */
}

/* Estilos para el contenedor de la lista de tareas */
.task-list-container {
  font-family: 'Noto Sans', sans-serif; /* Fuente de texto */
  display: flex;                /* Elementos se muestran en una fila */
  flex-wrap: wrap;              /* Permite que los elementos se ajusten automáticamente a diferentes tamaños */
  justify-content: space-between; /* Distribuye el espacio disponible de manera uniforme entre los elementos */
  width: 100%;                  /* Ancho del 100% */
  margin-top: 140px;            /* Margen superior de 100px */
}

/* Estilos para la tabla de tareas */
.task-table {
  margin-left: 85px;            /* Margen izquierdo de 85px */
  width: 50%;                   /* Ancho del 50% */
  border-collapse: collapse;    /* Colapsa los bordes de las celdas de la tabla */
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.404); /* Sombra alrededor de la tabla */
  background-color: #fff;       /* Color de fondo blanco */
  padding-left: 40px;           /* Espacio interno izquierdo de 40px */
}

/* Estilos para los encabezados de columna y celdas de la tabla */
.task-table th,
.task-table td {
  padding: 12px 16px;           /* Espacio interno de las celdas */
  border-bottom: 1px solid #e0e0e0; /* Borde inferior de 1px sólido */
  text-align: left;             /* Alineación del texto a la izquierda */
}

/* Estilos para el primer elemento de cada fila de la tabla */
.task-table th:first-child,
.task-table td:first-child {
  padding-left: 24px;           /* Espacio interno izquierdo de 24px */
}

/* Estilos para el último elemento de cada fila de la tabla */
.task-table th:last-child,
.task-table td:last-child {
  padding-right: 24px;          /* Espacio interno derecho de 24px */
}

/* Estilos para los encabezados de columna de la tabla */
.task-table thead th {
  background-color: #001e2d;    /* Color de fondo del encabezado */
  color: #fff;                  /* Color del texto del encabezado */
}

/* Estilos para las filas de la tabla al pasar el cursor sobre ellas */
.task-table tbody tr:hover {
  background-color: #f9f9f9;    /* Color de fondo al pasar el cursor */
}

/* Estilos para los elementos de tipo checkbox */
input[type='checkbox'] {
  accent-color: #00ff00;        /* Color de acento para el checkbox */
  cursor: pointer;              /* Cursor en forma de puntero */
}

/* Estilos para el ícono de eliminación */
.delete-icon {
  color: red;                   /* Color rojo */
  cursor: pointer;              /* Cursor en forma de puntero */
  margin-left: 20px;            /* Margen izquierdo de 20px */
}

/* Estilos para el ícono de eliminación al pasar el cursor sobre él */
.delete-icon:hover {
  color: darkred;               /* Color rojo oscuro al pasar el cursor */
}

/* Estilos para el contenedor de botones */
.boton-container{
  position: absolute;           /* Posición absoluta */
  left:30px;                    /* Posición izquierda de 10px */
  top: 200px;
}

/* Estilos para el botón de agregar */
.add-button {
  margin-left: 70px;            /* Margen izquierdo de 70px */
  background-color: #00425b;    /* Color de fondo */
  color: #fff;                  /* Color del texto */
  padding: 8px 12px;            /* Espacio interno del botón */
  border: none;                 /* Sin borde */
  border-radius: 5px;           /* Borde redondeado */
  cursor: pointer;              /* Cursor en forma de puntero */
  margin-bottom: 10px;          /* Margen inferior de 10px */
  margin-top: 20px;             /* Margen superior de 20px */
}

/* Estilos para el ícono dentro del botón de agregar */
.add-button i {
  margin-right: 5px;            /* Margen derecho de 5px */
}

.no-tasks {
    padding: 0px;
    font-size: 18px;
    color: #888;
    font-weight: bold;
  }

/* Estilos para dispositivos móviles */
@media (max-width: 767px) {
  .gradient{
    height: 800px;              /* Altura de 800px */
  }
  .task-list-container{
    margin-top: 80px;           /* Margen superior de 57px */
    display: inline-block;      /* Elementos se muestran en línea */
  }
  .task-table {
    margin-left: 0;             /* Margen izquierdo de 0 */
    width: 100%;                /* Ancho del 100% */
    padding-left: 0;            /* Espacio interno izquierdo de 0 */
  }

  .task-table th,
  .task-table td {
    padding: 8px 12px;          /* Espacio interno de las celdas */
    font-size: 14px;            /* Tamaño de fuente de 14px */
  }

  .task-table th:first-child,
  .task-table td:first-child {
    padding-left: 16px;         /* Espacio interno izquierdo de 16px */
  }

  .task-table th:last-child,
  .task-table td:last-child {
    padding-right: 16px;        /* Espacio interno derecho de 16px */
  }
  .boton-container{
    left: -60px;                /* Posición izquierda de -60px */
    top: 137px ;
  }
  .add-button{
    padding: 10px;              /* Espacio interno del botón de 10px */
  }
}

/* Estilos para tabletas */
@media (min-width: 768px) and (max-width: 1250px) {
  .gradient{
    height: 820px;              /* Altura de 820px */
  }
  .task-table {
    margin-left: 0;             /* Margen izquierdo de 0 */
    width: 100%;                /* Ancho del 100% */
  }
  .add-button{
    padding: 10px;              /* Espacio interno del botón de 10px */
  }
  .boton-container{
    left: -60px;                /* Posición izquierda de -60px */
  }
}
</style>
