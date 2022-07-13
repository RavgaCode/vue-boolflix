<template>
  <section>
    <div class="container">
      <!-- Searchbar -->
      <div>
        <input
          @keyup.enter="getMovies()"
          type="text"
          placeholder="Che film stai cercando?"
          v-model="text"
        />
      </div>
      <div>
        <button type="submit" @click="getMovies">Cerca</button>
      </div>
      <!-- Movie section -->
      <div class="row">
        <div class="col-4" v-for="(movie, index) in movieList" :key="index">
          <!-- Foto poster del film -->
          <img
            :src="`https://image.tmdb.org/t/p/w342${movie.backdrop_path}`"
            alt=""
          />
          <h2>{{ movie.title }}</h2>
          <h3>{{ movie.original_title }}</h3>
          <!-- Language flag con placeholder in caso di bandiera mancante-->
          <img
            v-if="availableFlags.includes(movie.original_language)"
            :src="require(`../assets/img/${movie.original_language}.svg`)"
            alt=""
            class="language-flag"
          />
          <img
            v-else
            src="../assets/img/euro.svg"
            alt=""
            class="language-flag"
          />
          <h5>
            <!-- Stars rating -->
            <span
              v-for="(element, index) in 5"
              :key="index"
              :class="{
                'yellow-star': element <= starsRating(movie.vote_average),
              }"
              ><i class="fa-solid fa-star"></i
            ></span>
          </h5>
        </div>
      </div>
      <!-- Series section -->
      <div class="row">
        <div class="col-4" v-for="(serie, index) in seriesList" :key="index">
          <!-- Foto poster della serie -->
          <img
            :src="`https://image.tmdb.org/t/p/w342${serie.backdrop_path}`"
            alt=""
          />
          <h2>{{ serie.name }}</h2>
          <h3>{{ serie.original_name }}</h3>
          <!-- Language flag con placeholder in caso di bandiera mancante-->
          <img
            v-if="availableFlags.includes(serie.original_language)"
            :src="require(`../assets/img/${serie.original_language}.svg`)"
            alt=""
            class="language-flag"
          />
          <img
            v-else
            src="../assets/img/euro.svg"
            alt=""
            class="language-flag"
          />
          <h5>
            <!-- Stars rating -->
            <span
              v-for="(element, index) in 5"
              :key="index"
              :class="{
                'yellow-star': element <= starsRating(serie.vote_average),
              }"
              ><i class="fa-solid fa-star"></i
            ></span>
          </h5>
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
      seriesList: [],
      availableFlags: ["en", "it"],
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
      // Ottengo l'array per le serie TV con lo stesso metodo precedente
      axios
        .get(
          `https://api.themoviedb.org/3/search/tv?api_key=b585fd483e3e78376584f5c26e640090&query=${this.text}`
        )
        .then((result) => {
          this.seriesList = result.data.results;
        });
      // Resetto la searchbar
      this.text = "";
    },
    starsRating(number) {
      let rating = Math.round(number / 2);
      return rating;
    },
  },
};
</script>

<style lang="scss">
@import "../style/style.scss";
.language-flag {
  width: 30px;
}
.yellow-star {
  color: yellow;
}
</style>
