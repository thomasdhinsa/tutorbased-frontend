<script>
import axios from "axios";
// import SavedModal from "../components/SavedModal.vue";
export default {
  // components: { SavedModal },
  data: function () {
    return {
      user: {},
      newReviewParams: {},
      editReviewParams: {},
      errors: [],
      // showModal: false,
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
    showReview: function (review) {
      console.log(review);
      this.currentReview = review;
      document.querySelector("#review-body").showModal();
    },
    updateReview: function () {
      this.editReviewParams.review = this.user.id;
      // do I need another parameter to make sure that the correct user is editing their review?
      console.log(this.editReviewParams);
      axios
        .patch("/reviews", this.editReviewParams)
        .then((response) => {
          console.log(response.data, "the button works?");
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
      <button v-on:click="showReview(review)">Edit Review</button>
      <!-- <h1>here's the modal</h1>
      <SavedModal />
    </div>
    <SavedModal v-show="showModal" />
    <div class="save-btn">
      <button @click="showModal = true">Save</button> -->
    </div>

    <!-- <SavedModal v-show="showModal" @close-modal="showModal = false" /> -->
  </div>
</template>
