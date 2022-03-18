<script>
import axios from "axios";
export default {
  data: function () {
    return {
      user: {},
      // currentUserId: localStorage.user_id (this is for the search method)
    };
  },
  created: function () {
    axios.get(`/users/${this.$route.params.id}`).then((response) => {
      console.log("Teacher Show:", response.data);
      this.user = response.data;
    });
  },
};
</script>

<template>
  <div class="users-show">
    <h1>{{ user.name }}</h1>
    <img v-bind:src="user.image_url" alt="" />
    <h2>{{ user.education }}</h2>
    <h3>{{ user.bio }}</h3>
    <!-- <p>{{ user.earned_reviews }}</p> -->
    <div v-for="review in user.earned_reviews" v-bind:key="review.id">
      <p>{{ review.body }}</p>
      <p>{{ review.rating }}</p>
    </div>
    <form action=""></form>
  </div>
</template>
