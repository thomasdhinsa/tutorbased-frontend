<script>
import axios from "axios";
// import SavedModal from "./components/SavedModal.vue";
export default {
  // components: { SavedModal },
  data: function () {
    return {
      user: {},
      newReviewParams: {},
      editReviewParams: {},
      errors: [],
      userId: localStorage.user_id,
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
    },
    updateReview: function (review) {
      this.editReviewParams.user_id = this.userId;
      console.log(this.editReviewParams);
      axios
        .patch(`/reviews/${review.id}`, this.editReviewParams)
        .then((response) => {
          console.log(response.data, "Review updated");
          this.$router.go();
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(this.errors);
        });
    },
    destroyReview: function (review) {
      if (confirm("Would you like to delete this review?")) {
        axios.delete(`/reviews/${review.id}`).then((response) => {
          console.log("review deleted", response.data);
        });
      }
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
  <div class="users-show">
    <h1>{{ user.name }}</h1>
    <img v-bind:src="user.image_url" alt="" />
    <h2>Education: {{ user.education }}</h2>
    <h3>A Little Something About Me: {{ user.bio }}</h3>
    <h4>The Subjects I Specialize In: {{ user.subjects }}</h4>
    <h4>Preferred Contact Options: {{ user.preferred_contact }}</h4>
    <h4>My Area of Common Operation: {{ user.zipcode }}</h4>
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
      <p>Rating: {{ review.rating }}</p>
      <button
        v-on:click="editReviewParams.id = review.id"
        v-if="userId == review.user_id"
        type="button"
        class="btn btn-primary"
        data-bs-toggle="modal"
        data-bs-target="#editReviewModal"
      ></button>
      <button v-on:click="destroyReview(review)" v-if="userId == review.user_id">Delete Review</button>
      <p>Press Blue Button(if available) to Edit Review</p>
      <!-- <p>{{ userId }}</p>
      <p>{{ review.user_id }}</p> -->
    </div>
    <!-- Modal -->
    <div
      class="modal fade"
      id="editReviewModal"
      tabindex="-1"
      aria-labelledby="editReviewModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="editReviewModalLabel">Edit Review</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <form v-on:submit.prevent="updateReview(editReviewParams)">
              <ul>
                <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
              </ul>
              <div>
                <label>Rating:</label>
                <input type="text" v-model="editReviewParams.rating" />
              </div>
              <div>
                <label>Body:</label>
                <input type="text" v-model="editReviewParams.body" />
              </div>
              <input class="btn btn-primary" type="submit" value="Update" />
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
