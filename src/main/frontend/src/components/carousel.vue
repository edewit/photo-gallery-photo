<template>
  <div>
    <section v-if="errored">
      <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
      <p>{{errored}}</p>
    </section>

    <section v-else>
      <div v-show="loading">Loading...</div>
      <div v-show="!loading" class="carouselbox">
        <button class="prev" @click="navigate(-1)" title="Previous">◀</button>
        <ol>
          <li v-for="photo in photos" :key="photo.id">
            <img :src="'/photos/' + photo.id" />
            <div class="caption">{{photo.name}}</div>
          </li>
        </ol>
        <button class="next" @click="navigate(1)" title="Next">▶</button>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      loading: true,
      errored: "",
      photos: null,
      items: null,
      counter: 0
    };
  },
  methods: {
    navigate: function(direction) {
      const current = this.items[this.counter];
      current.classList.remove("current");
      this.counter = this.counter + direction;
      if (direction === -1 && this.counter < 0) {
        this.counter = this.items.length - 1;
      }
      if (direction === 1 && !this.items[this.counter]) {
        this.counter = 0;
      }
      this.items[this.counter].classList.add("current");
      this.$emit("photoId", this.photos[this.counter].id);
    }
  },
  mounted() {
    axios
      .get("/photos")
      .then(response => {
        this.photos = response.data;
      })
      .catch(error => {
        this.errored = error.message;
      })
      .finally(() => {
        this.loading = false;

        this.items = this.$el.querySelectorAll("li");
        this.navigate(0);
      });
  }
};
</script>

<style scoped>
.carouselbox {
  display: flex;
  align-items: stretch;
  width: 100%;
}
.carouselbox button {
  border: none;
}
ol {
  flex-grow: 1;
  text-align: center;
}
li {
  opacity: 0;
  transition: 1s;
  list-style: none;
  display: none;
}
li.current {
  opacity: 1;
  transition: 1s;
  display: block;
}
.caption {
  font-style: italic;
}
</style>