<template>
  <div id="login">
    <bare-bones-header />
    <form class="center" @submit.prevent="login">
      <h1>Please Sign In</h1>
      <div class="style" role="alert" v-if="invalidCredentials">
        Invalid username and password!
      </div>
      <div class="style" role="alert" v-if="this.$route.query.registration">
        Thank you for registering, please sign in.
      </div>
      <div class="form-input-group">
        <label class="style" for="username">Username</label>
        <input
          type="text"
          id="username"
          v-model="user.username"
          required
          autofocus
        />
      </div>
      <div class="form-input-group">
        <label class="style" for="password">Password</label>
        <input type="password" id="password" v-model="user.password" required />
      </div>
      <button class="style button" type="submit">Sign in</button>
      <p>
        <router-link class="style" :to="{ name: 'register' }"
          >Need an account? Sign up.</router-link
        >
      </p>
    </form>
  </div>
</template>

<script>
import authService from "../services/AuthService";
import BareBonesHeader from "../components/BareBonesHeader.vue";

export default {
  name: "login",
  components: { BareBonesHeader },
  data() {
    return {
      user: {
        username: "",
        password: "",
      },
      invalidCredentials: false,
    };
  },
  methods: {
    login() {
      authService
        .login(this.user)
        .then((response) => {
          if (response.status == 200) {
            this.$store.commit("SET_AUTH_TOKEN", response.data.token);
            this.$store.commit("SET_USER", response.data.user);
            this.$router.push("/");
          }
        })
        .catch((error) => {
          const response = error.response;

          if (response.status === 401) {
            this.invalidCredentials = true;
          }
        });
    },
  },
};
</script>

<style scoped>
.form-input-group {
  margin-bottom: 1rem;
  font-weight: bold;
  color: rgba(80, 71, 66);
}
label {
  margin-right: 0.5rem;
}
h1 {
  font-family: "Big Shoulders Display", cursive;
  color: rgba(80, 71, 66);
}
.style {
  font-family: "Poppins", cursive;
  font-weight: bold;
}
.center {
  position: absolute;
  top: 40%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.button {
  background-color: #4caf50; /* Green */
  border: none;
  color: white;
  padding: 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 12px;
}
</style>