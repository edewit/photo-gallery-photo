<template>
  <photoForm v-if="!photoId" v-on:photoId="onSubmit"></photoForm>
  <upload v-else :photoId="photoId" v-on:photoId="onSubmit"></upload>
</template>

<script>
import photoForm from "./photoForm.vue";
import upload from "./upload.vue";

export default {
  data() {
    return {
      photoId: undefined
    };
  },
  components: { photoForm, upload },
  methods: {
    onSubmit(photoId) {
      this.photoId = photoId;
    }
  }
}
</script>