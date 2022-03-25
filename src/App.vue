<script>
export default {
  data: function () {
    return {
      isLoggedIn: !!localStorage.jwt,
      flashMessage: null,
      // cssProps: {
      //   backgroundImage: `url(${require("@/../../public/img/school.png")})`,
      // },
    };
  },
  watch: {
    $route: function () {
      this.isLoggedIn = !!localStorage.jwt;
      this.flashMessage = localStorage.flashMessage;
      localStorage.removeItem("flashMessage");
      this.current_user_id = localStorage.getItem("user_id");
    },
  },
};
</script>
<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">TutorBased</a>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="/">Home</a>
          </li>
          <li class="nav-item">
            <a v-if="!isLoggedIn" class="nav-link" href="/signup">Signup</a>
          </li>
          <li class="nav-item dropdown">
            <a
              class="nav-link dropdown-toggle"
              href="#"
              id="navbarDropdown"
              role="button"
              data-bs-toggle="dropdown"
              aria-expanded="false"
            >
              Login/Update Profile
            </a>
            <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
              <li><a v-if="!isLoggedIn" class="dropdown-item" href="/login">Login</a></li>
              <li><a v-if="isLoggedIn" class="dropdown-item" href="/users/:id/edit">Update Profile</a></li>
              <li><hr class="dropdown-divider" /></li>
              <li><a v-if="isLoggedIn" class="dropdown-item" href="/logout">Logout</a></li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
  </nav>
  <div class="container">
    <router-view />
  </div>
</template>
<style>
body {
  background-image: url("/public/img/school.png");
}

.card img {
  object-fit: cover;
  height: 500px;
}
</style>
