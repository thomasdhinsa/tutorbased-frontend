<script>
import axios from "axios";
export default {
  data: function () {
    return {
      user: {},
      editUserParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get("/users/" + this.$route.params.id).then((response) => {
      console.log("users show", response);
      this.user = response.data;
      this.editUserParams = this.photo;
    });
  },
  methods: {
    updateProfile: function (user) {
      axios
        .patch("/users/" + user.id, this.editUserParams)
        .then((response) => {
          console.log("user update", response);
          this.$router.push("/users");
        })
        .catch((error) => {
          console.log("user update error", error.response);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="edit-profile">
    <h1>Edit Prrofile</h1>
    <form v-on:submit.prevent="updateProfile(user)">
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      Name:
      <input type="text" v-model="editUserParams.name" />
      Email:
      <input type="text" v-model="editUserParams.email" />
      Image:
      <input type="text" v-model="editUserParams.image_url" />
      Bio:
      <input type="text" v-model="editUserParams.url" />
      <input type="submit" value="Update" />
    </form>
  </div>
  <img v-bind:src="user.image_url" v-bind:alt="user.name" />
  <router-link v-bind:to="`/users/${user.id}/edit`">Edit Profile</router-link>
  <router-link to="/users">Back to all users</router-link>
</template>
