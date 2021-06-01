<template>
  <div class="movie-detail">
    <h2>{{ movie.Title }}</h2>
    <p>{{ movie.Released }}</p>
    <img :src="movie.Poster" alt="movie poster" class="featured-img" />
    <p>{{ movie.Genre }}</p>
    <p class="ratings">
      Ratings: <span>{{ movie.Ratings[0].Value }}</span> (imdb),
      <span>{{ movie.Ratings[1].Value }}</span> (Rotten Tomatoes),
      <span>{{ movie.Ratings[2].Value }}</span> (Metacritic),
    </p>
    <p><span>Description:</span> {{ movie.Plot }}</p>
    <p><span>Director:</span> {{ movie.Director }}</p>
    <p><span>Actors:</span> {{ movie.Actors }}</p>
  </div>
</template>

<script>
import { ref, onBeforeMount } from "vue";
import { useRoute } from "vue-router";
import env from "@/env.js";

export default {
  setup() {
    const movie = ref({});
    const route = useRoute();

    onBeforeMount(() => {
      fetch(
        `http://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`
      )
        .then((res) => res.json())
        .then((data) => {
          movie.value = data;
        });
    });

    return {
      movie,
    };
  },
};
</script>

<style lang="scss">
.movie-detail {
  padding: 16px;

  h2 {
    color: #fff;
    font-size: 28px;
    font-weight: 600;
    margin-bottom: 16px;
  }

  .featured-img {
    display: block;
    max-width: 200px;
    margin-bottom: 16px;
    margin-left: 10px;
  }

  p {
    color: #fff;
    font-size: 18px;
    line-height: 1.4;
    padding: 10px;
  }

  span {
      color: #42b883;
  }

  .ratings {
    color: #fff;

    span {
        color: #42b883;
    }
  }
}
</style>
