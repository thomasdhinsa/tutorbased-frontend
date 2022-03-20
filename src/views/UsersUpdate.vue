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
    axios.get("/users/${this.$route.params.id}").then((response) => {
      console.log("user to edit", response);
      this.user = response.data;
      this.editUserParams = this.user;
    });
  },
  methods: {
    updateProfile: function () {
      axios
        .patch("/users/${this.user.id}", this.user)
        .then((response) => {
          console.log("user update:", response.data);
          this.$router.push("/users/${user.id}");
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
      <input type="text" v-model="editUserParams.name" />
      <label>Email:</label>
      <input type="email" v-model="editUserParams.email" />
      <label>Password:</label>
      <input type="password" v-model="editUserParams.password" />
      <label>Password confirmation:</label>
      <input type="password" v-model="editUserParams.password_confirmation" />
      <label>education:</label>
      <input v-if="is_teacher" type="text" v-model="editUserParams.education" />
      <label>zipcode:</label>
      <input v-if="is_teacher" type="text" v-model="editUserParams.zipcode" />
      <label>bio:</label>
      <input v-if="is_teacher" type="text" v-model="editUserParams.bio" />
      <label>Subject(s):</label>
      <input v-if="is_teacher" type="text" v-model="editUserParams.subjects" />
      <label>preferred_contact:</label>
      <input v-if="is_teacher" type="text" v-model="editUserParams.preferred_contact" />
      <label>image_url:</label>
      <input type="text" v-model="editUserParams.image_url" />

      <input type="submit" value="Update" />
    </form>
  </div>
  <img v-bind:src="user.image_url" alt="" />

  <router-link v-bind:to="`/users/${user.id}/edit`">Edit Profile</router-link>
  <router-link to="/">Back to all users</router-link>
</template>
