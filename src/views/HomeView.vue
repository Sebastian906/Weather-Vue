<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input 
        type="text" 
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Busca una ciudad, estado o país."
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004e71]"
      />
      <ul 
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
        v-if="openWeatherResults.length"
      >
        <li 
          v-for="result in openWeatherResults"
          :key="result.id"
          @click="selectCity(result)"
          class="py-2 cursor-pointer hover:bg-weather-primary"
        >
          {{ result.name }}, {{ result.state ? result.state + ',' : '' }} {{ result.country }}
        </li>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const searchQuery = ref(""); 
const queryTimeout = ref(null);
const openWeatherResults = ref([]); // Lista de resultados
const apiKey = import.meta.env.VITE_OPENWEATHER_API_KEY; 

// Función para buscar ciudades usando Geocoding API
const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value.trim() !== "") {
      try {
        const result = await axios.get(
          `https://api.openweathermap.org/geo/1.0/direct?q=${searchQuery.value}&limit=5&appid=${apiKey}`
        );
        
        // Guardar resultados con nombre, estado y país
        openWeatherResults.value = result.data.map(location => ({
          id: `${location.lat}-${location.lon}`, // Usar latitud-longitud como ID único
          name: location.name,
          state: location.state || null, // Algunos lugares no tienen estado
          country: location.country,
          lat: location.lat,
          lon: location.lon
        }));
      } catch (error) {
        console.error("Error al buscar ciudades:", error);
        openWeatherResults.value = []; // Limpiar en caso de error
      }
    } else {
      openWeatherResults.value = [];
    }
  }, 300);
};

</script>
