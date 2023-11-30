<script setup>
import { useRouter } from 'vue-router';
import { useAuthStore } from '@/stores/auth';

const auth = useAuthStore();
const router = useRouter();

const onLogout = () => {
  auth.logout()

  router.push('/login')
}

router.beforeEach((to, from, next) => {
  // Guard for Restricted route
  if (to.path === '/restricted' && !auth.username) {
    // Show alert instead of redirecting
    window.alert('Please log in to access Restricted page');
    next(false); // Prevent navigation
  } else {
    next();
  }
});
</script>

<template>
  <header>
      <div class="navbar">
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
        <RouterLink to="/restricted">Restricted</RouterLink>
      </div>
      <div class="auth">
        <p class="username" v-if="auth.username">{{ auth.username }}</p>
        <div v-if="auth.username">
          <button class="login-nav" @click="onLogout()">Logout</button>
        </div>
        <div v-else>
          <RouterLink class="login-nav" to="/login">Login</RouterLink>
        </div>
      </div>
  </header>
  <body>

  </body>

  <RouterView></RouterView>
</template>

<style>

body {
  width: 100%;
  height: 100vh;
  background-image: url(./assets/preview.jpg);
  background-size: cover;
  background-position: bottom;
  backdrop-filter: brightness(60%);
  z-index: -99;
}

.navbar {
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  gap: 20px;
  margin: 20px 50px;
}

a {
  color: white;
}

.login-nav {
  border: 1px solid white ;
  border-radius: 3px;
  padding: 1px 5px 3px 5px;
}

.username {
  color: white;
  font-size: 16px;
}

.auth {
  position: absolute;
  top: 30px;
  right: 70px;
  display: flex;
  gap: 20px;
}
</style>