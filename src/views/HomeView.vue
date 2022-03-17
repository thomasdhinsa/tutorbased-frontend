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
    <h1>Here Are Some Of The Great Teachers</h1>
    <button v-on:click="indexTeachers()">Load Teachers</button>
    <div v-for="user in users" v-bind:key="user.teacher_id">
      <h2>{{ user.name }}</h2>
      <img v-bind:src="user.image_url" v-bind:alt="user.name" />
    </div>
  </div>
</template>

<style></style>
