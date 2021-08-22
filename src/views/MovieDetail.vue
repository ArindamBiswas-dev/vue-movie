<template>
  <section class="flex justify-center">
    <div class="w-full max-w-6xl py-6">
      <h2 class="font-semibold text-left text-2xl underline italic">
        MovieDetails Page
      </h2>
      <div class="py-12 space-y-4 px-2">
        <div class="grid grid-cols-1 md:grid-cols-2 items-start">
          <div>
            <img
              :src="movieData.Poster"
              alt="movie_poster"
              class="h-80 w-80 object-cover rounded-lg shadow-md"
            />
            <h2 class="text-2xl font-semibold py-4">{{ movieData.Title }}</h2>
          </div>
          <div>
            <h2 class="text-xl font-medium pb-4">About</h2>
            <p class="text-base max-w-lg text-justify">{{ movieData.Plot }}</p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { onBeforeMount, onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';
import env from '@/env.js';
export default {
  name: 'MovieDetail',
  setup() {
    const route = useRoute();
    let movieData = ref({});
    const imdbId = route.params.id;

    onBeforeMount(() => {
      axios
        .get(
          `http://www.omdbapi.com/?apikey=${env.api_key}&i=${imdbId}&plot=full`
        )
        .then((response) => {
          movieData.value = response.data;
          // console.log(movieData.value);
          // console.log(movieData.value.Title);
        })
        .catch((error) => console.log(error));
    });
    onMounted(() => console.log('onmounted in MovieDetails'));

    return { movieData };
  },
};
</script>

<style scoped></style>
