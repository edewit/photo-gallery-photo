<template>
  <div>
    <section v-if="errored">
      <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
      <p>{{errored}}</p>
    </section>

    <section v-else>
      <div v-show="loading">Loading...</div>
      <div v-show="!loading" class="carouselbox" ref="box">
        <div class="buttons">
          <button class="prev" v-on:click="navigate(-1)">
            ◀
            <span class="offscreen">Previous</span>
          </button>
          <button class="next" v-on:click="navigate(1)">
            <span class="offscreen">Next</span> ▶
          </button>
        </div>
        <ol>
          <li v-for="photo in photos" v-bind:key="photo.id">
            <img v-bind:src="'/photos/' + photo.id" />
            <div class="caption">{{photo.name}}</div>
          </li>
        </ol>
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

        this.items = this.$el.querySelectorAll('li');
        this.navigate(0);
      });
  }
};
</script>

<style scoped>
.buttons {
  padding: 5px 0;
  background: #eee;
  text-align: center;
  z-index: 10;
  position: relative;
}
.carouselbox {
  width: 200px;
  border: 1px solid #ccc;
  box-shadow: 2px 2px 10px #ccc;
}
.carouselbox button {
  border: none;
}
.offscreen {
  position: absolute;
  left: -2000px;
}
ol {
  padding: 10px;
}
img {
  width:100%;
  max-width: 200px;
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
  text-align: center;
  font-style: italic;
}
</style>