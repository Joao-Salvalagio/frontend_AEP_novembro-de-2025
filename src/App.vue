<template>
  <div id="app">
    
    <LoginView v-if="!isLoggedIn" @login-success="handleLoginSuccess" />

    <MainDashboard v-else :user="currentUser" @logout="handleLogout" />

  </div>
</template>

<script setup>
import { ref } from 'vue';

import LoginView from './views/LoginView.vue';
import MainDashboard from './components/MainDashboard.vue';

const isLoggedIn = ref(false);
const currentUser = ref(null);

function handleLoginSuccess(userData) {
  currentUser.value = userData; 
  isLoggedIn.value = true;      
  
  localStorage.setItem('userData', JSON.stringify(userData));
}

function handleLogout() {
  currentUser.value = null;
  isLoggedIn.value = false;
  localStorage.removeItem('userData');
}

const storedUser = localStorage.getItem('userData');
if (storedUser) {
  try {
    currentUser.value = JSON.parse(storedUser);
    isLoggedIn.value = true;
  } catch (e) {
    localStorage.removeItem('userData');
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;700&family=Exo+2:wght@600;800&display=swap');
:root {
  --color-green-dark: #00796B;
  --color-green-medium: #00AD9A;
  --color-green-neon: #33FFC7;
  --color-text-dark: #222222;
  --color-text-light: #ffffff;
  --color-bg-card: #ffffff;
  --color-bg-1: #F0FFF9;
  --color-bg-2: #E6FFFA;
}
* { box-sizing: border-box; }
body {
  margin: 0; padding: 0;
  font-family: 'Poppins', sans-serif;
  color: var(--color-text-dark);
  background: linear-gradient(-45deg, var(--color-bg-1), var(--color-bg-2), var(--color-bg-1));
  background-size: 400% 400%;
  animation: gradient-bg 20s ease infinite;
}
@keyframes gradient-bg { 0% { background-position: 0% 50%; } 50% { background-position: 100% 50%; } 100% { background-position: 0% 50%; } }
#app { display: flex; flex-direction: column; min-height: 100vh; }
</style>