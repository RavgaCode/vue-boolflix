<template>
  <section>
    <div class="ms_container">
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
      <h2 v-show="movieList.length > 1">Film</h2>
      <div class="slide-container">
        <div class="film-card" v-for="(movie, index) in movieList" :key="index">
          <!-- Front card - Poster e Titolo-->
          <div class="front-card">
            <!-- Foto poster del film -->
            <img
              :src="`https://image.tmdb.org/t/p/w342${movie.poster_path}`"
              alt=""
              class="front-cover"
            />
            <h2>{{ movie.title }}</h2>
          </div>
          <!-- Back card - Info -->
          <div class="back-card">
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
      </div>
      <!-- Series section -->
      <h2 v-show="seriesList.length > 1">Serie TV</h2>
      <div class="slide-container">
        <div
          class="film-card"
          v-for="(serie, index) in seriesList"
          :key="index"
        >
          <!-- Front card - Poster e Titolo-->
          <div class="front-card">
            <!-- Foto poster della serie -->
            <img
              :src="`https://image.tmdb.org/t/p/w342${serie.poster_path}`"
              alt=""
              class="front-cover"
            />
            <h2>{{ serie.name }}</h2>
          </div>
          <!-- Back card - Info -->
          <div class="back-card">
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
section {
  background-color: lightgrey;
  margin-top: 150px;
  height: calc(100vh - 150px);
  overflow: auto;
}
.slide-container {
  display: flex;
  justify-content: space-between;
  flex-wrap: nowrap;
  overflow-x: auto;
  width: 100%;
}
.film-card {
  width: calc($main-container-width / 4 - 1rem);
  margin-right: 1rem;
  flex-shrink: 0;
  position: relative;

  &:hover .back-card {
    display: block;
  }
}
.back-card {
  display: none;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: white;
  width: 100%;
  height: 100%;
}
.front-cover {
  max-width: 100%;
}
.language-flag {
  width: 30px;
}
.yellow-star {
  color: yellow;
}
</style>
