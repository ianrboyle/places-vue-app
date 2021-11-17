<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h2>{{ message }}</h2>
    <div v-for="place in places" :key="place.id">
      <h3>{{ place.name }}</h3>
      <p>{{ place.address }}</p>
      <button v-on:click="showPlace(place)">Show me More!</button>
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
    <dialog id="place-details">
      <form method="dialog">
        <h1>Recipe Info:</h1>
        <p>
          Name:
          <input type="text" v-model="currentPlace.name" />
        </p>
        <p>
          Address:
          <input type="text" v-model="currentPlace.address" />
        </p>
        <button v-on:click="updatePlace(currentPlace)">Update Place</button>
        <!-- <button v-on:click="destroyPlace(currentPlace)">Delete</button> -->
        <button>Close</button>
      </form>
    </dialog>
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
      currentPlace: {},
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
    showPlace: function (place) {
      axios.get("http://localhost:3000/places/" + place.id, place).then((response) => {
        this.currentPlace = place;
        document.querySelector("#place-details").showModal();
        console.log("succes1", response.data);
      });
    },
  },
};
</script>
