<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      teachers: [],
    };
  },
  mounted: function () {
    this.indexTeachers();
  },

  methods: {
    indexTeachers: function () {
      axios.get("/users").then((response) => {
        console.log("teachers index", response);
        this.teachers = response.data;
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
  </div>
</template>

<style></style>
