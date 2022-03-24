<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newSessionParams: {},
      errors: [],
    };
  },
  methods: {
    submit: function () {
      axios
        .post("/sessions", this.newSessionParams)
        .then((response) => {
          axios.defaults.headers.common["Authorization"] = "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          localStorage.setItem("user_id", response.data.user_id);
          localStorage.setItem("flashMessage", "Successfully logged in!");
          this.$router.push("/");
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = ["Invalid email or password."];
          this.email = "";
          this.password = "";
        });
    },
  },
};
</script>

<template>
  <div class="card bg-white text-black" style="width: 550px">
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
  <div class="login">
    <form v-on:submit.prevent="submit()">
      <h1>Login</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Email:</label>
        <input type="email" v-model="newSessionParams.email" />
      </div>
      <div>
        <label>Password:</label>
        <input type="password" v-model="newSessionParams.password" />
      </div>
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>
