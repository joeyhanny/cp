<template>
  <div id="app">
    <Navigation class="nav" role="header"/>
    <div class="page" role="main">
      <router-view  />
    </div>
    <div class="footer" role="footer">
      <div v-if="this.$root.$data.rCurrentUser.role == 'admin'">
        GitHub: <a target="_blank" href="https://github.com/joeyhanny/cp">https://github.com/joeyhanny/cp</a>
    </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navigation from "@/components/Navigation.vue";
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      recipeName: '',
      currentRecipe: ''
    }
  },
  components: {
    Navigation
  },
  created() {
    this.updateCurrent();
    this.resumeLogin();
  },
  methods: {
    updateCurrent() {
      this.$root.$data.currentRecipe = '';
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
};
</script>

<style>

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.page {
  padding-top: 11vh;
  min-height: 95vh; /*100 - min-height of the footer*/
}

.nav {
  position: fixed;
  z-index: 999;
  width: 100%;
}

h1 {
  font-size: 40px;
  margin: 15px;
}

h2 {
  font-size: 30px;
  margin-bottom: 10px;
  text-decoration: underline;
}

h3 {
  font-size: 25px;
}

h1, h2, h3, li, p {
  color: #041E42;
}

button {
  background-color: #041E42;
  color: white;
  border-radius: 3px;
  padding: 5px;
  margin: 5px;
}

input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

.footer {
  min-height: 5vh;
  background-color: #041E42;
  color: #FFFFFF;
  text-align: right;
  padding-left: 20px;
  padding-right: 20px;
  padding-top: 1vh;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}

.footer a, router-link {
  color: #FFFFFF;
  text-decoration: none;
}

.footer a:hover,router-link:hover {
  text-decoration: underline;
}
</style>
