<template>
  <div id="app" class="app-container">
    <Header v-if="isLoggedIn" :user="currentUser" @logout="logout" />
    <main>
      <LoginForm v-if="!isLoggedIn" @login="handleLogin" />
      <Dashboard v-else :user="currentUser" />
    </main>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import LoginForm from './components/LoginForm.vue';
import Dashboard from './components/Dashboard.vue';

export default {
  name: 'App',
  components: {
    Header,
    LoginForm,
    Dashboard
  },
  data() {
    return {
      isLoggedIn: false,
      currentUser: null
    };
  },
  mounted() {
    const savedUser = localStorage.getItem('atiw_user');
    if (savedUser) {
      this.currentUser = JSON.parse(savedUser);
      this.isLoggedIn = true;
    }
  },
  methods: {
    handleLogin(userData) {
      this.currentUser = userData;
      this.isLoggedIn = true;
      localStorage.setItem('atiw_user', JSON.stringify(userData));
    },
    logout() {
      this.isLoggedIn = false;
      this.currentUser = null;
      localStorage.removeItem('atiw_user');
    }
  }
};
</script>

<style>
.app-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
</style>