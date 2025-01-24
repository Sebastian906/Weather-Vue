<template>
    <header class="sticky top-0 bg-weather-primary shadow-lg">
        <nav class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6">
            <RouterLink :to="{ name: 'home' }">
                <div class="flex items-center gap-3">
                    <i class="fa-solid fa-sun text-2xl"></i>
                    <p class="text-2xl font-bold">The Local Weather</p>
                </div>
            </RouterLink>
            <div class="flex gap-3 flex-1 justify-end">
                <i class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer" @click="toggleModal"></i>
                <i class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer"></i>
            </div>
            <BaseModal :modalActive="modalActive" @close-modal="toggleModal">
                <div class="text-black">
                    <h1 class="text-2xl mb-1">Información:</h1>
                    <p class="mb-4">
                        The Local Weather le permite ver el clima actual y la previsión del tiempo para los próximos 7 días.
                    </p>
                    <h2 class="text-2xl">Como funciona:</h2>
                    <ol class="list-decimal list-inside mb-4">
                        <li>
                            Busca tu ciudad escrbiendo su nombre dentro de la barra de búsqueda.
                        </li>
                        <li>
                            Selecciona tu ciudad de la lista de resultados. 
                            Esto lo redireccionará al clima actual de la ciudad.
                        </li>
                        <li>
                            Rastrea la ciudad al darle click en el ícono de '+' en 
                            la esquina superior derecha. Esto guardar'a la ciudad para 
                            revisarla más tarde en la página principal.
                        </li>
                    </ol>

                    <h2 class="text-2xl">Remover una ciudad</h2>
                    <p>
                        Si ya no desea rastrear una ciudad, simplemente seleccione 
                        la ciudad en la página principal. En el fondo de la página, 
                        habrá una opción para eliminar la ciudad.
                    </p>
                </div>
            </BaseModal>
        </nav>
    </header>
</template>

<script setup>
import { RouterLink } from "vue-router";
import BaseModal from "./BaseModal.vue";
import { ref } from "vue";

const modalActive = ref(null);
const toggleModal = () => {
    modalActive.value = !modalActive.value;
}

</script>

