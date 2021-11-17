<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h2>{{ message }}</h2>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to my Places App.",
      places: [],
    };
  },
  created: function () {
    this.placesIndex();
  },
  methods: {
    placesIndex: function () {
      axios.get("http://localhost:3000/places").then((response) => {
        this.places = response.data;
        console.log("Success!", response.data);
      });
    },
  },
};
</script>
