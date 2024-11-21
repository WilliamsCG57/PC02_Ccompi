<template>
  <div class="date-range-filter">
    <q-btn
      color="primary"
      icon="calendar_today"
      label="Filtrar por fecha de estreno"
      @click="toggleDatePicker"
      flat
    />

    <!-- Popup para seleccionar fechas -->
    <q-popup-proxy
      v-model="datePickerVisible"
      transition-show="scale"
      transition-hide="scale"
    >
      <div class="date-picker-container">
        <!-- Calendario para fecha de inicio -->
        <q-date
          v-model="startDate"
          mask="YYYY-MM-DD"
          label="Fecha de inicio"
          :min="minDate"
          :max="endDate ? endDate : null"
          @input="updateDateRange"
        />
        <!-- Calendario para fecha final -->
        <q-date
          v-model="endDate"
          mask="YYYY-MM-DD"
          label="Fecha final"
          :min="startDate ? startDate : null"
          @input="updateDateRange"
        />
      </div>
    </q-popup-proxy>
  </div>
</template>

<script>
export default {
  name: "MoviesDateFilter",
  data() {
    return {
      datePickerVisible: false, // Controla la visibilidad del selector de fechas
      startDate: "", // Fecha de inicio seleccionada
      endDate: "", // Fecha final seleccionada
      minDate: "1900-01-01", // Fecha mínima permitida para seleccionar
    };
  },
  methods: {
    toggleDatePicker() {
      // Muestra u oculta el selector de fechas cuando se hace clic en el botón
      this.datePickerVisible = !this.datePickerVisible;
    },
    updateDateRange() {
      // Emitir las fechas seleccionadas al componente padre
      this.$emit("update:selectedDateRange", {
        startDate: this.startDate,
        endDate: this.endDate,
      });
    },
  },
};
</script>

<style scoped>
.date-range-filter {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.q-btn {
  margin-bottom: 10px;
}

.date-picker-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>
