<template>
  <div id="app">
    <div class="header">
      <Header @login="triggerLogin" />
    </div>
    <div class="content">
      <router-view/>
    </div>
    <div class="footer">
      <Footer/>
    </div>
    <div class="login">
      <Login/>
    </div>
    <div class="register">
      <Register/>
    </div>
  </div>
</template>

<script>
import Header from './components/header/Header'
import Footer from './components/footer/Footer'
import Login from './components/login/Login'
import Register from './components/register/Register'
import axios from 'axios'

export default {  
  name: 'App',
  components: {
    Header,
    Footer,
    Login,
    Register
  },
  data () {
    return {
      login: false,
      userInfo: {}
    }
  },
  methods: {
    triggerLogin() {
      this.login = !this.login
    },
    getUserInfo() {
      let url = `api/v1/auth/${this.infoUsuario._id}/me`
      axios.get(url)
      .then(response => {
        this.userInfo = response.data.data
         this.$store.commit('infoUsuario', this.userInfo)
      })
      .catch(() =>{
        console.log('Error getting user info')
      })
    }
  },
  mounted() {
     if (this.$store.state.token !== null) {
      this.$store.commit('infoUsuario', JSON.parse(localStorage.getItem('infoUsuario')))
      this.$store.commit('login', true)
      this.getUserInfo()

    }
  },
  computed: {
    infoUsuario() {
      return this.$store.state.infoUsuario
    }
  }
}
</script>

<style lang="scss">
@import url('../node_modules/vue2-animate/dist/vue2-animate.min.css');
html {
  font-family: 'Bangers', cursive;
}

#app {
  position: relative;
}

.login {
position: fixed;
width: 500px;
height: 100px;
top: 25%;
left: 50%;
margin-top: -100px; /* Negative half of height. */
margin-left: -250px; /* Negative half of width. */
z-index: 5;
}
</style>
