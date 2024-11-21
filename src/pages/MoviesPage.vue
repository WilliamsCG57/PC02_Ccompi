<template>
  <div class="movies-page q-ml-md q-mr-xl">
    <!-- Componente para el filtro de búsqueda -->
    <div class="movies-filter q-ml-md q-mr-xl">
      <MoviesFilter v-model="query" />
    </div>

    <!-- Componente para el filtro de rango de votación -->
    <div class="vote-range-filter q-ml-md q-mr-xl">
      <VoteRangeButtons v-model:selectedRange="voteRange" />
    </div>

    <!-- Componente para el filtro de conteo de votos -->
    <div class="vote-count-filter q-ml-md q-mr-xl">
      <VoteCountRangeButtons v-model:selectedCountRange="voteCountRange" />
    </div>

    <!-- Componente para la lista de películas -->
    <div class="movies-list">
      <MoviesList :movies="filteredMovies" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import MoviesFilter from "src/components/movies/MoviesFilter.vue";
import VoteRangeButtons from "src/components/movies/VoteRangeButtons.vue";
import VoteCountRangeButtons from "src/components/movies/VoteCountRangeButtons.vue"; // Nuevo componente
import MoviesList from "src/components/movies/MoviesList.vue";

export default {
  name: "MoviesPage",
  components: {
    MoviesFilter,
    VoteRangeButtons,
    VoteCountRangeButtons,
    MoviesList,
  },
  data() {
    return {
      query: "", // Búsqueda por texto
      voteRange: { min: 1, max: 10 }, // Rango de votación inicial (1-10)
      voteCountRange: { min: 0, max: 6000 }, // Rango de conteo de votos inicial (0-6000)
      movies: [], // Lista completa de películas
    };
  },
  computed: {
    // Combina todos los filtros (texto, rango de votación y conteo de votos)
    filteredMovies() {
      return this.movies.filter(
        (movie) =>
          // Filtro por búsqueda
          movie.title.toLowerCase().includes(this.query.toLowerCase()) &&
          // Filtro por rango de votación
          movie.vote_average >= this.voteRange.min &&
          movie.vote_average <= this.voteRange.max &&
          // Filtro por rango de conteo de votos
          movie.vote_count >= this.voteCountRange.min &&
          movie.vote_count <= this.voteCountRange.max
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
    this.fetchMovies(); // Obtener películas al montar el componente
  },
};
</script>

<style>
.movies-page {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.movies-filter,
.vote-range-filter {
  width: 25%;
}

.vote-count-filter {
  display: flex;
  flex-wrap: nowrap; /* Evita que los botones se envuelvan */
  gap: 0.5rem; /* Espaciado entre botones */
}
</style>
