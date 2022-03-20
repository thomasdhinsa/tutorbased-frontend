<script>
import axios from "axios";
export default {
  data: function () {
    return {
      user: {},
      newReviewParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/users/${this.$route.params.id}`).then((response) => {
      console.log("Teacher Show:", response.data);
      this.user = response.data;
    });
  },
  methods: {
    createReview: function () {
      this.newReviewParams.teacher_id = this.user.id;
      console.log(this.newReviewParams);
      axios
        .post("/reviews", this.newReviewParams)
        .then((response) => {
          console.log(response.data);
          this.user.earned_reviews.push(response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(this.errors);
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
      <input type="integer" v-model="newReviewParams.rating" required />
      <label>Body:</label>
      <input type="text" v-model="newReviewParams.body" required />
      <input type="submit" value="Create" />
    </form>
    <div v-for="review in user.earned_reviews" v-bind:key="review.id">
      <p>{{ review.body }}</p>
      <p>{{ review.rating }}</p>
    </div>
  </div>
</template>
