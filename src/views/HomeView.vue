<script>
import axios from "axios";
export default {
  data: function () {
    return {
      users: [],
      search: "",
      currentTeacher: 1,
      currentUserId: localStorage.getItem("user_id"),
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
    <div class="card bg-white text-black" style="width: 950px">
      <img
        src="https://media.istockphoto.com/photos/youre-the-best-teacher-picture-id1292825155?b=1&k=20&m=1292825155&s=170667a&w=0&h=5anTV1R9uXAkSokFjpSAtERuOKfCLWOOdpo2c8-Tzy0="
        class="card-img"
        alt=""
      />
      <div class="card-img-overlay">
        <h5 class="card-title">Welcome</h5>
        <p class="card-text">To</p>
        <p class="card-text">TUTORBASED</p>
      </div>
    </div>
    <h1>Here Are Some Of The Great Teachers</h1>
    <input type="text" v-model="search" placeholder="Search by Subject" />
    <div v-for="user in filteredUsers" v-bind:key="user.id">
      <div class="card" style="max-width: 540px">
        <img v-bind:src="user.image_url" class="card-img-top" alt="" />
        <div class="card-body">
          <h5 class="card-title">{{ user.name }}</h5>
        </div>
        <ul class="list-group list-group-flush">
          <li class="list-group-item">Specializations: {{ user.subjects }}</li>
          <li class="list-group-item">About Me: {{ user.bio }}</li>
        </ul>
        <div class="card-body">
          <router-link :to="`/users/${user.id}`" class="card-link">Click here to go to my page</router-link>
        </div>
      </div>
    </div>
  </div>
</template>
<style></style>
