<template>
  <q-input
    v-model="query"
    label="Buscar película"
    debounce="300"
    @input="$emit('search', query)"
  />
</template>

<script>
export default {
  data() {
    return {
      query: "", // Almacenamos el valor de búsqueda
    };
  },
};
</script>
