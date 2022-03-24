<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newUserParams: {},
      errors: [],
    };
  },
  methods: {
    submit: function () {
      axios
        .post("/users", this.newUserParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/login");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
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
  <div class="signup">
    <form v-on:submit.prevent="submit()">
      <h1>Signup</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Name:</label>
        <input type="text" v-model="newUserParams.name" />
      </div>
      <div>
        <label>Email:</label>
        <input type="email" v-model="newUserParams.email" />
      </div>
      <div>
        <label>Password:</label>
        <input type="password" v-model="newUserParams.password" />
      </div>
      <div>
        <label>Password confirmation:</label>
        <input type="password" v-model="newUserParams.password_confirmation" />
      </div>
      <div>
        <label>Are You A Teacher? (If Yes, Please Update Your Profile Directly After Signup)</label>
        <input type="checkbox" id="checkbox" v-model="newUserParams.is_teacher" />
        <label for="checkbox"></label>
      </div>
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>
