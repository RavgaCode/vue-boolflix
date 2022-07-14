<template>
  <div id="app">
    <TopHeader @searchMovie="getMovies" />
    <MainContent :movieList="movieList" :seriesList="seriesList" />
  </div>
</template>

<script>
import axios from "axios";
import TopHeader from "./components/TopHeader.vue";
import MainContent from "./components/MainContent.vue";
export default {
  name: "App",
  components: {
    TopHeader,
    MainContent,
  },
  data() {
    return {
      textToSearch: "",
      movieList: [],
      seriesList: [],
    };
  },
  methods: {
    getMovies(text) {
      this.textToSearch = text;
      console.log(this.textToSearch);
      // Ottengo l'array di film tramite API, utilizzando come query il testo digitato dall'utente
      axios
        .get(
          `https://api.themoviedb.org/3/search/movie?api_key=b585fd483e3e78376584f5c26e640090&query=${this.textToSearch}`
        )
        .then((result) => {
          this.movieList = result.data.results;
        });
      // Ottengo l'array per le serie TV con lo stesso metodo precedente
      axios
        .get(
          `https://api.themoviedb.org/3/search/tv?api_key=b585fd483e3e78376584f5c26e640090&query=${this.textToSearch}`
        )
        .then((result) => {
          this.seriesList = result.data.results;
        });
    },
  },
};
</script>

<style lang="scss">
@import "./style/style.scss";
#app {
}
</style>
