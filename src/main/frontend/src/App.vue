<template>
  <div class="app">
    <carousel></carousel>
    <hr />
    <br />
    <add></add>
  </div>
</template>

<script>
import carousel from "./components/carousel.vue";
import add from "./components/add.vue";

export default {
  name: "App",
  components: { carousel, add },
};
</script>

<style>
body {
  font-family: "Roboto";
}
</style>