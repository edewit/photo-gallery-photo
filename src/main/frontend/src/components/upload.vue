<template>
  <div class="file-upload">
    {{photoId}}
    <label for="upload" class="file-upload__label">Upload image</label>
    <input
      id="upload"
      class="file-upload__input"
      type="file"
      name="file-upload"
      @change="upload()"
      ref="upload"
    />
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: {
    photoId: Number
  },
  methods: {
    upload: function() {
      const file = this.$refs.upload.files[0];
      const formData = new FormData();
      formData.append("file", file);
      formData.append("id", this.photoId);
      /*eslint no-console: "off"*/
      axios
        .post("/photos/upload", formData, {
          headers: {
            "Content-Type": "multipart/form-data"
          }
        })
        .then(function() {
          console.log("SUCCESS!!");
        })
        .catch(function() {
          console.log("FAILURE!!");
        });
    }
  }
};
</script>

<style scoped>
.file-upload {
  position: relative;
  display: inline-block;
}

.file-upload__label {
  display: block;
  padding: 1em 2em;
  color: #fff;
  background: #222;
  border-radius: 0.4em;
  transition: background 0.3s;
}
.file-upload__label:hover {
  cursor: pointer;
  background: #000;
}

.file-upload__input {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  font-size: 1;
  width: 0;
  height: 100%;
  opacity: 0;
}
</style>