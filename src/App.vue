<template>
  <div id="app">
    <TopHeader />
    <MainContent />
  </div>
</template>

<script>
import TopHeader from "./components/TopHeader.vue";
import MainContent from "./components/MainContent.vue";
export default {
  name: "App",
  components: {
    TopHeader,
    MainContent,
  },
};
</script>

<style lang="scss">
@import "./style/style.scss";
#app {
}
</style>
