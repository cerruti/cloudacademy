<template>
  <div id="app">
    <CharacterProfile msg="Hello Vue" />
  </div>
</template>

<script>
import CharacterProfile from "./components/CharacterProfile";

export default {
  name: "App",
  components: {
    CharacterProfile,
  },
};
</script>
<style>
body {
  background: #000000;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
