<template>
  <div class="flex" id="menu">
    <div :class="screenWidth < minWidth ? 'flex space-between width-inherit' : ''">
      <div id="brand">
        <router-link to="/">
            <img @click="hideMenu" src="/images/logo.png" alt="Home">
        </router-link>
      </div>
      <div v-if="screenWidth < minWidth" class="menu-switch">
        <img :class="showMenu ? 'rotate-90' : ''" @click="switchMenuVisibility" src="/images/iconmonstr-menu-1-240.png" alt="Menu">
      </div>
    </div>
    <div :class="screenWidth > minWidth || showMenu ? 'flex links space-between' : 'hide'">
      <div id="menu-left">
        <div @click="hideMenu" id="tabs">
          <router-link to="/">
            <span class="menu-item">Home</span>
          </router-link>
          <router-link to="/recipes">
            <span class="menu-item" @click="reloadRecipes()">Recipes</span>
          </router-link>
          <router-link to="/about">
            <span class="menu-item">About</span>
          </router-link>
        </div>
      </div>
      <div @click="hideMenu" id="menu-right">
        <div id="tabs">
          <router-link to="/user">
              <span v-if="this.$root.$data.rCurrentUser != ''" class="menu-item">{{fullName}}</span>
                <span v-else class="menu-item">Log In</span>
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script scoped>
  export default {
    name: 'Navigation',
    data() {
    return {
        screenWidth: 0,
        minWidth: 700,
        showMenu: false,
      }
    },
    methods: {
      reloadRecipes() {
        this.$root.$data.currentRecipe = '';
        if (location.pathname.includes("recipes")) {
          //location.reload();
        }
      },
      handleResize() {
        this.screenWidth = window.innerWidth;
      },
      switchMenuVisibility() {
        this.showMenu = !this.showMenu;
      },
      hideMenu() {
        this.showMenu = false;
      }
    },
    computed: {
      fullName() {
        return this.$root.$data.rCurrentUser.firstName + " " + this.$root.$data.rCurrentUser.lastName;
      },
    },
    created() {
        window.addEventListener('resize', this.handleResize);
        this.handleResize();
    },
    destroyed() {
        window.removeEventListener('resize', this.handleResize);
    },
  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  #menu {
  }

  #menu-left, #menu-right, #menu {

    background-color: #041E42;
    padding: 10px;
    align-items: center;
  }

  img {
    width: 100px;
    height: auto;
  }

  .links {
    align-items: center;
  }

  .space-between {
    flex-grow: 1;
    justify-content: space-between;
  }

  .width-inherit {
    width: inherit;
  }

  .menu-switch img {
    height: 40px;
    width: 35px;
  }

  .rotate-90 {
    transform: rotate(90deg);
  }

  #tabs {

    height: inherit;
  }

  #tabs a {
    font-weight: bold;
    color: #808d9f;
    text-decoration: none;
  }

  #tabs a.router-link-exact-active {
    color: #fefefb;
  }

  .menu-item {
    font-size: 20px;
    margin: 10px;
  }

  .block {
    display: block;
  }

  .hide {
    display: none;
  }

  .flex {
    display: flex;
    flex-wrap: wrap;
  }
</style>
