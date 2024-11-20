<template>
  <q-page>
    <q-toolbar>
      <q-toolbar-title>Listado de Películas</q-toolbar-title>
      <q-input
        v-model="query"
        label="Buscar película"
        debounce="300"
        @input="fetchMovies"
      />
    </q-toolbar>
    <q-list>
      <q-item v-for="movie in movies" :key="movie.id">
        <q-item-section avatar>
          <img
            :src="`http://image.tmdb.org/t/p/w500${movie.poster_path}`"
            alt="poster"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ movie.title }}</q-item-label>
          <q-item-label caption>
            Votos: {{ movie.vote_average }} ({{ movie.vote_count }})
          </q-item-label>
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      movies: [],
      query: "",
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
                "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJhMzJlZjM0NThmOTdlNzY4NDViNzA0MjNmZmVkN2I5YyIsIm5iZiI6MTczMjA1NjgzMy4yMTI3MTAxLCJzdWIiOiI2NzNjZmRlZTRkNmRiMDBkOTNkNGRmOTciLCJzY29wZXMiOlsiYXBpX3JlYWQiXSwidmVyc2lvbiI6MX0.5NeNTeMIFHqZ6d60ZZyZQQDGq1wRb-io9CSzxClz9NQ", // Reemplaza con el token
            },
            params: {
              language: "es-PE",
              sort_by: "popularity.desc",
              query: this.query,
            },
          }
        );
        this.movies = response.data.results;
      } catch (error) {
        console.error(error);
      }
    },
  },
  mounted() {
    this.fetchMovies();
  },
};
</script>

<style>
.product-page {
  display: flex;
  justify-content: space-between;
}
.product-filter {
  width: 25%;
}
.product-list {
  width: 75%;
}
</style>
