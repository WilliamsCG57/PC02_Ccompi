<template>
  <div class="movies-page q-ml-md q-mr-xl">
    <!-- Componente para el filtro de búsqueda -->
    <div class="movies-filter q-ml-md q-mr-xl">
      <MoviesFilter v-model="query" />
    </div>
    <!-- Componente para la lista de películas -->
    <div class="movies-list">
      <MoviesList :movies="filteredMovies" />
    </div>
  </div>
</template>

<style>
.movies-page {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.movies-list {
  width: 75%;
}
.movies-filter {
  width: 25%;
}
</style>

<script>
import axios from "axios";
import MoviesFilter from "src/components/movies/MoviesFilter.vue";
import MoviesList from "src/components/movies/MoviesList.vue";

export default {
  name: "MoviesPage",
  components: { MoviesFilter, MoviesList },
  data() {
    return {
      query: "", // Término de búsqueda
      movies: [], // Lista completa de películas
    };
  },
  computed: {
    // Filtra las películas según el término de búsqueda
    filteredMovies() {
      if (!this.query) return this.movies; // Si no hay búsqueda, retorna todas las películas
      const searchQuery = this.query.toLowerCase();
      return this.movies.filter((movie) =>
        movie.title.toLowerCase().includes(searchQuery)
      );
    },
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
              language: "es-PE",
              sort_by: "popularity.desc",
            },
          }
        );
        this.movies = response.data.results || [];
      } catch (error) {
        console.error("Error al obtener películas:", error);
      }
    },
  },
  mounted() {
    this.fetchMovies(); // Cargar películas al montar
  },
};
</script>
