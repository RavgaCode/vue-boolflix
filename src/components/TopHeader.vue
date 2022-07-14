<template>
  <header>
    <div class="ms_container">
      <div class="flex-container">
        <h1>BOOLFLIX</h1>
        <div class="search-bar">
          <input
            @keyup.enter="$emit('searchMovie', text)"
            type="text"
            placeholder="Che film stai cercando?"
            v-model="text"
          />
          <button type="submit" @click="$emit('searchMovie', text)">
            Cerca
          </button>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "TopHeader",
  data() {
    return {
      text: "",
    };
  },
};
</script>

<style lang="scss" scoped>
@import "../style/style.scss";
header {
  height: 150px;
  width: 100%;
  background-color: black;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 10;
}

.flex-container {
  height: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
h1 {
  color: red;
  font-weight: 800;
}
</style>
