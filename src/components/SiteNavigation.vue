<template>
    <header class="sticky top-0 bg-weather-primary shadow-lg">
      <nav
        class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6"
      >
        <RouterLink :to="{ name: 'home' }">
          <div class="flex items-center gap-3">
            <i class="fa-solid fa-sun text-2xl"></i>
            <p class="text-2xl">La Météo Locale</p>
          </div>
        </RouterLink>
  
        <div class="flex gap-3 flex-1 justify-end">
          <i
            class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer"
            @click="toggleModal"
          ></i>
          <i
            class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer"
            @click="addCity"
            v-if="route.query.preview"
          ></i>
        </div>
  
        <BaseModal
          :modalActive="modalActive"
          @close-modal="toggleModal"
        >
        <div class="text-black">
            <h1 class="text-2xl mb-1">A propos :</h1>
            <p class="mb-4">
              La Météo Locale vous permet de rechercher la température actuelle et à venir de la ville de votre choix.
            </p>
            <h2 class="text-2xl">Comment ça marche :</h2>
            <ol class="list-decimal list-inside mb-4">
              <li>
                Cherchez votre ville en entrant son nom dans la barre de recherche.
              </li>
              <li>
                Selectionnez une ville parmi les résultats, cela vous montrera la température actuelle.
              </li>
              <li>
                Enregistrez la ville en cliquant sur l'icône "+" en haut à droite. Vous pourrez la retrouver plus tard sur la page d'accueil.
              </li>
            </ol>
  
            <h2 class="text-2xl">Supprimez une ville</h2>
            <p>
              Si vous ne voulez plus suivre une ville, sélectionnez simplemennt la ville dans la page d'accueil. En bas de la page, il y aura une option pour la supprimer.
            </p>
          </div>
        </BaseModal>
      </nav>
    </header>
  </template>
  
  <script setup>
  import { RouterLink, useRoute, useRouter } from "vue-router";
  import { uid } from "uid";
  import { ref } from "vue";
  import BaseModal from "./BaseModal.vue";
  
  const savedCities = ref([]);
  const route = useRoute();
  const router = useRouter();
const addCity = () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(
      localStorage.getItem("savedCities")
    );
  }

  const locationObj = {
    id: uid(),
    state: route.params.state,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lng: route.query.lng,
    },
  };

  savedCities.value.push(locationObj);
  localStorage.setItem(
    "savedCities",
    JSON.stringify(savedCities.value)
  );

  let query = Object.assign({}, route.query);
  delete query.preview;
  query.id = locationObj.id;
  router.replace({ query });
};

const modalActive = ref(null);
const toggleModal = () => {
  modalActive.value = !modalActive.value;
};
  </script>