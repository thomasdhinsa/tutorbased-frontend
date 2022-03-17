<script>
import axios from "axios";
export default {
  data: function () {
    return {
      teacher: {},
      // currentUserId: localStorage.user_id
    };
  },
  created: function () {
    axios.get(`/users/${this.$route.params.id}`).then((response) => {
      console.log("Teacher Show:", response.data);
      this.user = response.data;
    });
  },
  methods: {
    destroyReview: function () {
      if (confirm("Are you sure you want to delete this?")) {
        axios.delete(`/reviews/${this.review.id}`).then((response) => {
          console.log("Success", response.data);
          this.$router.push("/reviews");
        });
      }
    },
  },
};
</script>
