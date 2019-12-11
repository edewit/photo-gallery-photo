<template>
  <div class="app">
    <carousel></carousel>
    <hr />
    <br />
    <photoForm v-if="!photoId" v-on:photoId="onSubmit"></photoForm>
    <upload v-else :photoId="photoId" v-on:photoId="onSubmit"></upload>
  </div>
</template>

<script>
import carousel from "./components/carousel.vue";
import photoForm from "./components/photoForm.vue";
import upload from "./components/upload.vue";

export default {
  name: "App",
  data() {
    return {
      photoId: undefined
    };
  },
  components: { carousel, photoForm, upload },
  methods: {
    onSubmit(photoId) {
      this.photoId = photoId;
    }
  }
};
</script>

<style>
body {
  font-family: "Roboto";
}
</style>