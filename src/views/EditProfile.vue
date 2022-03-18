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
      this.editUserParams = this.user;
    });
  },
  methods: {
    editProfile: function (user) {
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
    <h1>Edit Profile</h1>
    <form v-on:submit.prevent="editProfile(user)">
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <label>Name:</label>
      <input type="text" v-model="newUserParams.name" />
      <label>Email:</label>
      <input type="email" v-model="newUserParams.email" />
      <label>Password:</label>
      <input type="password" v-model="newUserParams.password" />
      <label>Password confirmation:</label>
      <input type="password" v-model="newUserParams.password_confirmation" />
      <label>Is_teacher:</label>
      <input type="boolean" v-model="newUserParams.is_teacher" />
      <label>education:</label>
      <input v-if="is_teacher" type="text" v-model="newUserParams.education" />
      <label>zipcode:</label>
      <input v-if="is_teacher" type="text" v-model="newUserParams.zipcode" />
      <label>bio:</label>
      <input v-if="is_teacher" type="text" v-model="newUserParams.bio" />
      <label>Subject(s):</label>
      <input v-if="is_teacher" type="text" v-model="newUserParams.subjects" />
      <label>preferred_contact:</label>
      <input v-if="is_teacher" type="text" v-model="newUserParams.preferred_contact" />
      <label>image_url:</label>
      <input type="text" v-model="newUserParams.image_url" />
    </form>
  </div>
  <img v-bind:src="user.image_url" alt="" />
  <router-link v-bind:to="`/users/${user.id}/edit`">Edit Profile</router-link>
  <router-link to="/users">Back to all users</router-link>
</template>
