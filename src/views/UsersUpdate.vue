<script>
import axios from "axios";
export default {
  data: function () {
    return {
      user: {},
      currentUserId: localStorage.getItem("user_id"),
      editUserParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/users/${this.currentUserId}`).then((response) => {
      console.log("user to edit", response);
      this.user = response.data;
      this.editUserParams = this.user;
    });
  },
  methods: {
    destroyProfile: function (user) {
      if (confirm("Would you like to delete your account?")) {
        axios.delete(`/users/${user.id}`).then((response) => {
          console.log("User deleted", response.data);
          localStorage.removeItem("jwt");
          localStorage.removeItem("user_id");
          localStorage.setItem("flashMessage", "User successfully deleted");
          this.$router.push("/");
        });
      }
    },
    updateProfile: function () {
      axios
        .patch(`/users/${this.user.id}`, this.user)
        .then((response) => {
          console.log("user update:", response.data);
          localStorage.setItem("flashMessage", "User Successfully Updated");
          this.$router.push("/");
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
    <form v-on:submit.prevent="updateProfile()">
      <h1>Update Profile</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <label>Name:</label>
      <input type="text" v-model="user.name" />
      <label>Email:</label>
      <input type="email" v-model="user.email" />
      <label>Password:</label>
      <input type="password" v-model="user.password" />
      <label>Password confirmation:</label>
      <input type="password" v-model="user.password_confirmation" />
      <label>education:</label>
      <input v-if="user.is_teacher === true" type="text" v-model="user.education" />
      <label>zipcode:</label>
      <input v-if="user.is_teacher === true" type="text" v-model="user.zipcode" />
      <label>bio:</label>
      <input v-if="user.is_teacher === true" type="text" v-model="user.bio" />
      <label>Subject(s):</label>
      <input v-if="user.is_teacher === true" type="text" v-model="user.subjects" />
      <label>preferred_contact:</label>
      <input v-if="user.is_teacher === true" type="text" v-model="user.preferred_contact" />
      <label>image_url:</label>
      <input type="text" v-model="editUserParams.image_url" />

      <input type="submit" value="Update" />
    </form>
  </div>
  <img v-bind:src="user.image_url" alt="" />
  <button v-on:click="destroyProfile(user)" v-if="currentUserId == user.id">Delete Profile</button>
  <router-link v-bind:to="`/users/${user.id}/edit`">Edit Profile</router-link>
  <router-link to="/">Back to all users</router-link>
</template>
