<template>
  <section class="flex justify-center">
    <div class="w-full max-w-6xl py-6">
      <h2 class="font-semibold text-left text-2xl underline italic">
        Home Page
      </h2>
      <div class="flex justify-center py-4 mx-2">
        <CustomInput v-model.trim="searchInput" @search="search" />
      </div>
      <div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-5">
        <template v-for="movie in movies" :key="movie.imdbID">
          <router-link
            :to="'/moviedetail/' + movie.imdbID"
            class="p-2 rounded-lg shadow-xl overflow-hidden mx-2 my-3 border"
          >
            <div>
              <img
                :src="movie.Poster"
                alt="movie"
                class="h-32 w-full object-cover rounded-lg"
              />
              <h2 class="py-1">{{ movie.Title }}</h2>
            </div></router-link
          >
        </template>
      </div>
    </div>
  </section>
</template>

<script>
import { ref } from '@vue/reactivity';
import CustomInput from '@/components/CustomInput.vue';
import axios from 'axios';
import { onBeforeMount } from '@vue/runtime-core';
import env from '@/env.js';
// @ is an alias to /src
export default {
  name: 'Home',
  components: {
    CustomInput,
  },
  setup() {
    const searchInput = ref('');
    const movies = ref([]);

    onBeforeMount(() => {
      if (localStorage.getItem('movies')) {
        axios
          .get(
            `http://www.omdbapi.com/?apikey=${
              env.api_key
            }&s=${localStorage.getItem('movies')}`
          )
          .then((response) => {
            movies.value = response.data.Search;
            console.log(movies.value);
          })
          .catch((error) => console.log(error));
      }
      console.log('onmounted in Home');
    });

    function search() {
      axios
        .get(
          `http://www.omdbapi.com/?apikey=${env.api_key}&s=${searchInput.value}`
        )
        .then((response) => {
          movies.value = response.data.Search;
          localStorage.setItem('movies', searchInput.value);
          console.log(movies.value);
        })
        .catch((error) => console.log(error));
    }

    return {
      searchInput,
      search,
      movies,
    };
  },
};
</script>
