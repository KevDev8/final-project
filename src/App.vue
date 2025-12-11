<template>
  <div id="app" class="container py-4"> <!-- Organizes the home page as a container with some vertical padding -->
    <component :is="loggedIn ? 'DataGrid' : 'LoginPage'" 
                @login-success="handleLoginSuccess"
                :password="password"
    /> <!-- Checks if loggedIn is true or false and loads DataGrid or LoginPage accordingly, passes password as a prop for either of them,
            and listens for a login-sucess event from LoginPage to switch loggedIn to true through handleLoginSuccess -->
  </div>
</template>

<script>
import LoginPage from './components/LoginPage.vue'
import DataGrid from './components/DataGrid.vue'

export default {
  name: 'App',
  components: { LoginPage, DataGrid },
  data() {
    return {
      loggedIn: false,
      password: 'cardinals',
    }
  },
  methods: { handleLoginSuccess() { this.loggedIn = true } }
}
</script>
