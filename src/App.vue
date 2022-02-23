<template>
  <h1 class="title">{{ data.name }}</h1>
  <Picture :url="url" />
  <Description :stats="stats"/>
  <NavBar @previous-pokemon="prevPoke" @next-pokemon="nextPoke"/>
</template>

<script>

import Picture from './components/Picture.vue'
import Description from './components/Description.vue'
import NavBar from './components/NavBar.vue'

import { onMounted } from '@vue/runtime-core'
import {ref} from 'vue'

export default {
  name: 'App',
  components: {
    Picture,
    Description,
    NavBar
  },
  setup() {
    const pokemonId = ref(100);
    const data = ref({});
    const loading = ref(true);
    const error = ref(null);

    let url = ref("");
    let stats = ref([]);


    let fetchData = function(pokeId) {
      loading.value = true;
      return fetch(`https://pokeapi.co/api/v2/pokemon/${pokeId}/`, {
        method: 'get',
        headers: {
          'content-type': 'application/json'
        }
      })
      .then(res => {
        // a non-200 response code
        if (!res.ok) {
          // create error instance with HTTP status text
          const error = new Error(res.statusText);
          error.json = res.json();
          throw error;
        }

        return res.json();
      })
      .then(json => {
        // set the response data
        data.value = json;
        console.log(data.value);
      })
      .catch(err => {
        error.value = err;
        // In case a custom JSON error response was provided
        if (err.json) {
          return err.json.then(json => {
            // set the JSON response message
            error.value.message = json.message;
          });
        }
      })
      .then(() => {
        loading.value = false;
      });
    };

    /**
     * Décrémentation du pokemonId
     */
    let prevPoke = function() {
      if(pokemonId.value >= 1) {
        pokemonId.value = pokemonId.value - 1;
        return fetchData(pokemonId.value)
          .then(getImageUrl)
          .then(getStats);
      }
    }

    /**
     * Incrémentation du pokemonId
     */
    let nextPoke = function() {
      if(pokemonId.value >= 0) {
        pokemonId.value = pokemonId.value + 1;
         return fetchData(pokemonId.value)
          .then(getImageUrl)
          .then(getStats);
      }
    }

    /**
     * Récupération de l'image du pokemon
     */
    let getImageUrl = function() {
      url.value = data.value && data.value.sprites ? data.value.sprites.other.dream_world.front_default : '';
    }

    /**
     * Récupération des stats du pokemon
     */
    let getStats = function() {
      stats.value = data.value && data.value.stats ? data.value.stats : [];
    }

    /**
     * When application is mounted, download the first element from the API
     */
    onMounted(() => {
      return fetchData(pokemonId.value)
        .then(getImageUrl)
        .then(getStats);
    });

    return {
      pokemonId,
      data,
      loading,
      error,
      url,
      stats,
      prevPoke,
      nextPoke,
      getImageUrl,
      getStats
    }
  }
}
</script>

<style>
#app {
  display: flex;
  flex-wrap: wrap;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.title {
  width: 100%;
  text-align: center;
  height: 50px;
  text-transform: uppercase;
}
</style>
