<template>
  <div>
    <input v-model="message" placeholder="edit me" />
    <p v-on:click="greet()">Message is: {{ message }}</p>
  </div>
</template>

<script>
export default {
  name: 'Hello',
  data() {
    return {
      message: ''
    };
  },
  methods: {
    greet: function(event) {
      console.log('hello', this.message);
    }
  }
};
</script>

<style scoped>
h1 {
  padding: 0 0.25em;
}
</style>
