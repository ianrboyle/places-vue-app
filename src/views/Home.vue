<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h2>{{ message }}</h2>
    <div v-for="place in places" :key="place.id">
      <h3>{{ place.name }}</h3>
      <p>{{ place.address }}</p>
    </div>
    <h3>Create a new place!</h3>
    <form v-on:submit.prevent="createPlace()">
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Name:</label>
        <input type="text" v-model="newPlaceParams.name" />
      </div>
      <div>
        <label>Address:</label>
        <input type="text" v-model="newPlaceParams.address" />
      </div>
      <input type="submit" value="Submit" />
    </form>
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
      newPlaceParams: {},
      errors: [],
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
    createPlace: function () {
      axios
        .post("http://localhost:3000/places", this.newPlaceParams)
        .then(() => {
          this.$router.push("/places");
          console.log("Success!");
        })
        .catch((error) => console.log(error.response));
    },
  },
};
</script>
