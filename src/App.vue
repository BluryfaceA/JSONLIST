<script>
import Navbar from './components/Navbar.vue';

export default {
  name: 'App',
  components: {
    Navbar,
  },
  data() {
    return {
      nombre: '',
      arrayDatos: [], // Array vacío para almacenar los datos de la API
      valor: 10, // Número de elementos por página
      tamañoarray: 0, // Tamaño total del array de datos
      contador: 0, // Número total de páginas
      valorpag: 1, // Página actual
      inicio: 0, // Índice inicial de los datos a mostrar
      fin: 10, // Índice final de los datos a mostrar
      Buscar: '', // Término de búsqueda
    };
  },
  computed: {
    // Computa los datos filtrados con la búsqueda
    MostrarFiltrados() {
      return this.arrayDatos
        .filter(item => item.title.toLowerCase().includes(this.Buscar.toLowerCase())) // Aplica el filtro
        .slice(this.inicio, this.fin); 
    }
  },
  methods: {
    // Función para cargar los datos desde la API
    async Array() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/posts');
        this.arrayDatos = await response.json();
        this.tamañoarray = this.arrayDatos.length;
        this.contador = Math.ceil(this.tamañoarray / this.valor);
      } catch (error) {
        console.error('Error fetching todos:', error);
      }
    },

    // Método que actualiza el término de búsqueda
    buscar(busqueda) {
      this.Buscar = busqueda; // Actualizamos el término de búsqueda
      this.Array();
    },

    // Función para avanzar a la siguiente página
    add() {
      if (this.fin < this.tamañoarray) {
        this.inicio += this.valor;
        this.fin = Math.min(this.fin + this.valor, this.tamañoarray);
        this.valorpag++;
      }
    },

    // Función para retroceder a la página anterior
    restar() {
      if (this.inicio > 0) {
        this.inicio -= this.valor;
        this.fin = this.inicio + this.valor;
        this.valorpag--;
      }
    },

    // Función para recargar los datos y reiniciar la paginación
    reloadTodos() {
      this.inicio = 0;
      this.fin = this.valor;
      this.valorpag = 1;
      this.Buscar = ''
      this.Array()
    },
  },
  mounted() {
    // Cargar los datos cuando el componente esté montado
    this.Array();
  }
};
</script>



<template>
  <div id="app">
    <Navbar @Buscar="buscar" />
  </div>
  
  <div class="container">
    <h1 class="text-primary">Hola, Bootstrap!</h1>

    <!-- Tabla que muestra los datos de todos -->
    <table class="table table-bordered">
      <thead class="table-dark">
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Título</th>
          <th scope="col">Contenido</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="todo in MostrarFiltrados" :key="todo.id">
          <td>{{ todo.id }}</td>
          <td>{{ todo.title }}</td>
          <td>{{ todo.body }}</td>
        </tr>
      </tbody>
    </table>
  </div>

  <nav aria-label="Page navigation example">
    <ul class="pagination justify-content-end">
      <li v-if="valorpag !== 1" class="page-item disabled">
        <button type="button" @click="restar()" class="btn btn-danger">&laquo;</button>
      </li>
      <li class="page-item"><a class="page-link" href="#">{{ valorpag }}</a></li>
      <p style="margin-top: 13px;"> .... </p>
      <li class="page-item"><a class="page-link" href="#" style="margin-left: 10px;"> {{ contador }}</a></li>
      <li class="page-item">
        <button v-if="valorpag < contador" type="button" @click="add()" class="btn btn-primary">&raquo;</button>
      </li>
    </ul>

    <button @click="reloadTodos" class="btn btn-warning mb-3">Recargar Datos</button>
  </nav>

  {{ nombre }}
</template>


<style>
table {
  margin-top: 25px;
}

.page-item {
  margin-right: 10px;
}
</style>