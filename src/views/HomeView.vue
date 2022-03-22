<script>
import axios from "axios";
export default {
  data: function () {
    return {
      users: [],
      search: "",
    };
  },
  mounted: function () {
    axios.get("/users").then((response) => {
      console.log("Teachers Index:", response.data);
      this.users = response.data;
    });
  },
  methods: {
    indexTeachers: function () {
      console.log("This is the teachers array");
      axios.get("http://localhost:3000/users").then((response) => {
        console.log("teachers index", response);
        this.users = response.data;
      });
    },
  },
  computed: {
    filteredUsers: function () {
      return this.users.filter((user) => {
        return user.subjects.toLowerCase().match(this.search.toLowerCase());
      });
    },
  },
};
</script>
<template>
  <div class="teachers-index">
    <h1>Here Are Some Of The Great Teachers</h1>
    <input type="text" v-model="search" placeholder="Search by subject or name:" />
    <div v-for="user in filteredUsers" v-bind:key="user.id">
      <img v-bind:src="user.image_url" alt="" />
      <h2>{{ user.name }}</h2>
      <h3>{{ user.bio }}</h3>
    </div>
  </div>
</template>
<style></style>
