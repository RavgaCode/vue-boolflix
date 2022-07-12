<template>
  <section>
    <div class="container">
      <div>
        <input
          type="text"
          placeholder="Che film stai cercando?"
          v-model="text"
        />
      </div>
      <div>
        <button type="submit" @click="getMovies">Cerca</button>
      </div>
      <div class="row">
        <div class="col-4" v-for="(movie, index) in movieList" :key="index">
          <h2>{{ movie.title }}</h2>
          <h3>{{ movie.original_title }}</h3>
          <h4>{{ movie.original_language }}</h4>
          <h5>{{ movie.vote_average }}</h5>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";
export default {
  name: "MainContent",
  data() {
    return {
      text: "",
      movieList: [],
    };
  },
  methods: {
    getMovies() {
      // Ottengo l'array di film tramite API, utilizzando come query il testo digitato dall'utente
      axios
        .get(
          `https://api.themoviedb.org/3/search/movie?api_key=b585fd483e3e78376584f5c26e640090&query=${this.text}`
        )
        .then((result) => {
          this.movieList = result.data.results;
        });
      // Resetto la searchbar
      this.text = "";
    },
  },
};
</script>

<style lang="scss">
@import "../style/style.scss";
</style>
