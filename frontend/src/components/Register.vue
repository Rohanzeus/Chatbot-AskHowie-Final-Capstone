<template>
  <div id="register" class="text-center">
     <div id="form-box">
      <form class="form-register" @submit.prevent="register">
        <h1 id="header" class="h3 mb-3 font-weight-normal">Create Account</h1>
        <div class="alert alert-danger" role="alert" v-if="registrationErrors">
          {{ registrationErrorMsg }}
        </div>
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
        <div id="confirmedPassword-group">
          <input
            type="password"
            id="confirmPassword"
            class="form-control"
            placeholder="Confirm Password"
            v-model="user.confirmPassword"
            required
          />
        </div>
        <router-link class="haveAccount" :to="{ name: 'login' }">Have an account?</router-link>
        <button id="button" class="form-control" type="submit">
          Create Account
        </button>
      </form>
    </div>  
  </div>
</template>

<script>
import authService from '../services/AuthService';

export default {
  name: 'register',
    mounted() {
    this.$nextTick(() => {
      this.$refs.username.focus();
    })
  },
  data() {
    return {
      user: {
        username: '',
        password: '',
        confirmPassword: '',
        role: 'user',
      },
      registrationErrors: false,
      registrationErrorMsg: 'There were problems registering this user.',
    };
  },
  methods: {
    register() {
      if (this.user.password != this.user.confirmPassword) {
        this.registrationErrors = true;
        this.registrationErrorMsg = 'Password & Confirm Password do not match.';
      } else {
        authService
          .register(this.user)
          .then((response) => {
            if (response.status == 201) {
              this.$router.push({
                path: '/login',
                query: { registration: 'success' },
              });
            }
          })
          .catch((error) => {
            const response = error.response;
            this.registrationErrors = true;
            if (response.status === 400) {
              this.registrationErrorMsg = 'Bad Request: Validation Errors';
            }
          });
      }
    },
    clearErrors() {
      this.registrationErrors = false;
      this.registrationErrorMsg = 'There were problems registering this user.';
    },
  },
};
</script>

<style>

</style>
