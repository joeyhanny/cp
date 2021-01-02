<template>
  <div class="user">
    <div v-if="this.$root.$data.rCurrentUser == ''">
      <div class="login-card">
        <div v-if="loginScreen">
          <h1>Log In</h1>
          <form class="pure-form">
            <fieldset>
              <legend>Login</legend>
              <input placeholder="username" v-model="usernameLogin">
              <input type="password" placeholder="password" v-model="passwordLogin">
            </fieldset>
            <fieldset>
              <button type="submit" class="pure-button pure-button-primary" @click.prevent="login">Login</button>
            </fieldset>
          </form>
          <div @click="toggleLogin">Register</div>
        </div>
        <div v-else>
          <h1>Register</h1>
          <form class="pure-form">
            <fieldset>
              <legend>Register for an account</legend>
              <input placeholder="first name" v-model="firstName">
              <input placeholder="last name" v-model="lastName">
            </fieldset>
            <fieldset>
              <input placeholder="username" v-model="username">
              <input type="password" placeholder="password" v-model="password">
            </fieldset>
            <fieldset>
              <button type="submit" class="pure-button pure-button-primary" @click.prevent="register">Register</button>
            </fieldset>
          </form>
          <div @click="toggleLogin">Login</div>
        </div>
      </div>
    </div>
    <div v-else>
      <h1>User</h1>
      <button type="button" class="pure-button pure-button-primary" @click.prevent="logout">Log Out</button>
      <div v-if="this.$root.$data.rCurrentUser.role == 'admin'">
        <hr/>
        <router-link to="/admin" tag="button">Admin</router-link>
      </div>
    </div>

  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';
export default {
  name: 'User',
  data() {
    return {
      loginScreen: true,
      usernameLogin: '',
      passwordLogin: '',
      firstName: '',
      lastName: '',
      username: '',
      password: '',
    }
  },
  methods: {
    toggleLogin() {
      this.loginScreen = !this.loginScreen;
    },
    async register() {
    this.error = '';
    this.errorLogin = '';
    if (!this.firstName || !this.lastName || !this.username || !this.password)
      return;
    try {
      let response = await axios.post('/api/users', {
        firstName: this.firstName,
        lastName: this.lastName,
        username: this.username,
        password: this.password,
      });
      this.$root.$data.rCurrentUser = response.data.user;

      this.firstName = '';
      this.lastName = '';
      this.username = '';
      this.password = '';

    } catch (error) {
      this.$root.$data.rCurrentUser = "";
    }
  },
  async login() {
     this.error = '';
     this.errorLogin = '';
     if (!this.usernameLogin || !this.passwordLogin)
       return;
     try {
       let response = await axios.post('/api/users/login', {
         username: this.usernameLogin,
         password: this.passwordLogin,
       });
       this.$root.$data.rCurrentUser = response.data.user;

       this.usernameLogin = "";
       this.passwordLogin = "";
     } catch (error) {
       this.errorLogin = "Error: " + error.response.data.message;
       this.$root.$data.rCurrentUser = "";
       this.passwordLogin = "";
     }
   },
   async logout() {
      try {
        await axios.delete("/api/users");
        this.$root.$data.rCurrentUser = "";
      } catch (error) {
        this.$root.$data.rCurrentUser = "";
      }
    },
   async resumeLogin() {
    try {
      let response = await axios.get("/api/users");
      this.$root.$data.rCurrentUser = response.data.user;
      return true;
    }
    catch (error) {
      console.log(error);
    }
   }
  }
}

</script>

<style>
  .user {
    margin-left: 45px;
    margin-right: 45px;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }

  .login-card {

    text-align: left;
  }

  /* Tablet Styles */
  @media only screen and (min-width: 501px) and (max-width: 960px) {

  }

  /* Small Desktop Styles */
  @media only screen and (min-width: 961px) {

  }
</style>
