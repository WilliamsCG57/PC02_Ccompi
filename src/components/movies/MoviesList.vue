<template>
  <h5>Listado de Películas</h5>
  <div class="movies-list q-ml-md q-mr-xl">
    <div class="movies-grid q-ml-md q-mr-xl">
      <div class="movie-item" v-for="movie in movies" :key="movie.id">
        <MoviesItem :movie="movie" />
      </div>
    </div>
  </div>
</template>

<style>
.movies-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr); /* Tres columnas ajustables */
  grid-gap: 100px; /* Espaciado entre elementos */
}
</style>

<script>
import MoviesItem from "src/components/movies/MoviesItem.vue"; // Importamos el componente MoviesItem

export default {
  name: "MoviesList",
  components: { MoviesItem },
  data() {
    return {
      movies: [], // Arreglo de películas
    };
  },
  mounted() {
    this.cargarPeliculas(); // Cargar películas al montar el componente
  },
  methods: {
    cargarPeliculas() {
      let endpointURL = "https://api.themoviedb.org/3/discover/movie"; // API para obtener las películas
      let token =
        "eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJhMzJlZjM0NThmOTdlNzY4NDViNzA0MjNmZmVkN2I5YyIsIm5iZiI6MTczMjA1NjgzMy4yMTI3MTAxLCJzdWIiOiI2NzNjZmRlZTRkNmRiMDBkOTNkNGRmOTciLCJzY29wZXMiOlsiYXBpX3JlYWQiXSwidmVyc2lvbiI6MX0.5NeNTeMIFHqZ6d60ZZyZQQDGq1wRb-io9CSzxClz9NQ"; // Reemplaza con tu API Key de TMDb
      let headers = {
        headers: {
          Authorization: "Bearer " + token,
          "Content-Type": "application/json",
        },
      };
      // Llamada al endpoint con Axios
      this.$api
        .get(endpointURL, headers)
        .then((response) => {
          this.movies = response.data.results; // Guardar los resultados en el arreglo de películas
          console.log("Películas cargadas:", this.movies);
        })
        .catch((error) => {
          console.error("Error al cargar películas:", error);
          this.$q.notify({
            message: "Ocurrió un error al cargar películas",
            color: "negative",
          });
        });
    },
  },
};
</script>
