<template>
  <div class="login-page">
    <div class="card">
      <form @submit.prevent="login">
        <div class="title">Connexion au compte privé</div>
        <input placeholder="Pseudo" type="text" v-model="username" />
        <br />
        <div class="password-input">
          <input
            placeholder="Mot De Passe"
            :type="passwordFieldType"
            v-model="password"
          />
          <span class="eye-icon" @click="togglePasswordVisibility">
            <i :class="showPassword ? 'fas fa-eye' : 'fas fa-eye-slash'"></i>
          </span>
        </div>
        <br />
        <button type="submit">Connexion</button>
      </form>
    </div>
  </div>
</template>

<script>
import { loginRest } from "./apis.js";

export default {
  data() {
    return {
      username: "",
      password: "",
      showPassword: false,
    };
  },
  computed: {
    passwordFieldType() {
      return this.showPassword ? "text" : "password";
    },
  },
  methods: {
    login() {
      loginRest(this.username, this.password)
        .then((response) =>
          this.$emit("onAuth", { ...response.data, secret: this.password })
        )
        .catch((error) => console.log("Login error", error));
    },
    togglePasswordVisibility() {
      this.showPassword = !this.showPassword;
    },
  },
};
</script>

<style>
.password-input {
  position: relative;
}

.password-input .eye-icon {
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-5%);
  cursor: pointer;
}
</style>
