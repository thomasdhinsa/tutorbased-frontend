<script>
import axios from "axios";
export default {
  data: function () {
    return {
      user: {},
      editReviewParams: {},
    };
  },
  created: function () {
    axios.get(`/users/${this.$route.params.id}`).then((response) => {
      console.log("Teacher Show:", response.data);
      this.user = response.data;
    });
  },
  submit: function () {
    axios.get("/reviews/${this.$route.params.id}").then((response) => {
      console.log("review to update", response);
      this.review = response.data;
      this.editReviewParams = this.review;
    });
  },
  methods: {
    createReview: function () {
      console.log("Time to make a review");
      axios.post("http://localhost:3000/reviews").then((response) => {
        console.log("The review has been created", response);
        this.users = response.data;
      });
    },
    updateReview: function () {
      axios
        .patch("/reviews/${this.review.id}", this.review)
        .then((response) => {
          console.log("review update:", response.data);
          this.$router.push("/reviews/${review.id}");
        })
        .catch((error) => {
          console.log("review update error", error.response);
          this.errors = error.response.data.errors;
        });
    },
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
    <form v-on:submit.prevent="createReview()">
      <h1>Create Review</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <label>Rating:</label>
      <input type="integer" v-model="review.rating" required />
      <label>Body:</label>
      <input type="text" v-model="review.body" required />
      <input type="submit" value="Create" />
    </form>
    <div v-for="review in user.earned_reviews" v-bind:key="review.id">
      <p>{{ review.body }}</p>
      <p>{{ review.rating }}</p>
    </div>
  </div>
</template>
