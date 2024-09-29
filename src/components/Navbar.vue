<template>
  <nav class="navbar navbar-expand-lg bg-body-tertiary">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">Navbar</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#">Home</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Link</a>
          </li>
          <li class="nav-item">
            <a class="nav-link disabled" aria-disabled="true">Curiosidades</a>
          </li>
        </ul>

        <form class="d-flex" role="search" @submit.prevent="emitirBusqueda">
          <input v-model="Buscar" class="form-control me-2" type="search" placeholder="Ingrese Nombre" aria-label="Search">
          <button class="btn btn-primary" type="submit">Buscar</button>
        </form>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'Navbar',
  data() {
    return {
      Buscar: '' // Este será el valor que emitimos al buscar
    };
  },
  methods: {
    emitirBusqueda() {
      this.$emit('Buscar', this.Buscar);
    }
  }
}
</script>

<style scoped>
/* Agrega estilos si es necesario */
</style>
