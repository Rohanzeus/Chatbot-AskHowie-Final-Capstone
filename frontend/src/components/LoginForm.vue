<template>
  <div id="login" class="text-center">
    <div id="form-box">
        <form class="form-signin" @submit.prevent="login">
            <h1 class="h3 mb-3 font-weight-normal">Please Sign In</h1>
            <div
                class="alert alert-danger"
                role="alert"
                v-if="invalidCredentials"
            >Invalid username and password!</div>
            <div
                class="alert alert-success"
                role="alert"
                v-if="this.$route.query.registration"
            >Thank you for registering, please sign in.</div>
            <div id="username-group">
              <label for="username" class="sr-only">Username</label>
              <input
                type="text"
                id="username"
                class="form-control"
                ref="username"
                placeholder="Username"
                v-model="user.username"
                required
                autofocus
            />
            </div>
            <div id="password-group">
              <label for="password" class="sr-only">Password</label>
              <input
                type="password"
                id="password"
                class="form-control"
                placeholder="Password"
                v-model="user.password"
                required
            />
            </div>

            <router-link :to="{ name: 'register' }">Need an account?</router-link>
            <button class="form-control" type="submit">Sign in</button>
        </form>
    </div>
    
  </div>
</template>

<script>
import authService from "../services/AuthService";

export default {
  name: "login",
  components: {},
  data() {
    return {
      user: {
        username: "",
        password: ""
      },
      invalidCredentials: false
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.$refs.username.focus();
    })
  },
  methods: {
    login() {
      authService
        .login(this.user)
        .then(response => {
          if (response.status == 200) {
            this.$store.commit("SET_AUTH_TOKEN", response.data.token);
            this.$store.commit("SET_USER", response.data.user);
            this.$router.push("/");
          }
        })
        .catch(error => {
          const response = error.response;

          if (response.status === 401) {
            this.invalidCredentials = true;
          }
        });
    }
  }
};
</script>

<style>

</style>