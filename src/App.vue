<template>
  <div id="app">
    <Main/>
  </div>
</template>

<script>
import Main from "./components/Main";

export default {
  name: "App",
  components: {
    Main
  }
};
</script>

<style>
#app {
  margin: 0;
  width: 100%;
  height: 100%;
}
body {
  margin: 0;
}
</style>
