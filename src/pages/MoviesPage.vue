<template>
  <div class="movies-page q-ml-md q-mr-xl">
    <div class="movies-list">
      <!-- Lista de películas -->
      <MoviesList :movies="movies" />
    </div>
  </div>
</template>

<style>
.movies-page {
  display: flex;
  justify-content: center; /* Centrar la lista */
}
.movies-list {
  width: 100%; /* Ancho completo para la lista */
}
</style>

<script>
import axios from "axios";
import MoviesList from "src/components/movies/MoviesList.vue"; // Componente MoviesList

export default {
  name: "MoviesPage",
  components: { MoviesList }, // Registramos el componente MoviesList
  data() {
    return {
      movies: [], // Lista de todas las películas
    };
  },
  methods: {
    async fetchMovies() {
      try {
        const response = await axios.get(
          "https://api.themoviedb.org/3/discover/movie",
          {
            headers: {
              Authorization:
                "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJhMzJlZjM0NThmOTdlNzY4NDViNzA0MjNmZmVkN2I5YyIsIm5iZiI6MTczMjA1NjgzMy4yMTI3MTAxLCJzdWIiOiI2NzNjZmRlZTRkNmRiMDBkOTNkNGRmOTciLCJzY29wZXMiOlsiYXBpX3JlYWQiXSwidmVyc2lvbiI6MX0.5NeNTeMIFHqZ6d60ZZyZQQDGq1wRb-io9CSzxClz9NQ",
            },
            params: {
              language: "es-PE", // Idioma español
              sort_by: "popularity.desc", // Orden por popularidad
            },
          }
        );
        this.movies = response.data.results || []; // Guardamos las películas en el estado
      } catch (error) {
        console.error("Error al obtener películas:", error);
      }
    },
  },
  mounted() {
    this.fetchMovies(); // Obtenemos las películas al montar el componente
  },
};
</script>
