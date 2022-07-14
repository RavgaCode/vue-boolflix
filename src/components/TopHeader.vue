<template>
  <header>
    <div class="ms_container">
      <div class="flex-container">
        <h1>BOOLFLIX</h1>
        <div class="search-bar">
          <input type="text" />
          <button>Cerca</button>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "TopHeader",
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
