<template>
  <div class="login-view">
    <TheHeader />
    
    <main class="login-content">
      
      <div class="login-card">
        <div class="card-glow-border"></div>
        <div class="card-content">

          <div class="toggle-buttons">
            <button 
              @click="isLoginMode = true" 
              :class="{ active: isLoginMode }"
            >
              Entrar
            </button>
            <button 
              @click="isLoginMode = false" 
              :class="{ active: !isLoginMode }"
            >
              Cadastrar-se
            </button>
          </div>

          <form v-if="isLoginMode" @submit.prevent="handleLogin" class="auth-form">
            <h2 class="form-title">Acessar sua Conta</h2>
            <div class="form-group"> <label for="login-email">E-mail:</label> <input type="email" id="login-email" class="form-input" v-model="loginData.email" required> </div>
            <div class="form-group"> <label for="login-senha">Senha:</label> <input type="password" id="login-senha" class="form-input" v-model="loginData.senha" required> </div>
            <button type="submit" class="form-button">Entrar</button>
            <div v-if="loginError" class="feedback-message error">{{ loginError }}</div>
          </form>

          <FormCadastro v-else /> 

        </div>
      </div>

    </main>

    <TheFooter />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
import TheHeader from '../components/TheHeader.vue';
import TheFooter from '../components/TheFooter.vue';
import FormCadastro from '../components/FormCadastro.vue'; 
const emit = defineEmits(['login-success']);
const isLoginMode = ref(true); 
const loginData = ref({ email: '', senha: '' });
const loginError = ref('');
const API_LOGIN_URL = 'http://localhost:8080/api/usuarios/login';
async function handleLogin() {
  loginError.value = ''; 
  try {
    const response = await axios.post(API_LOGIN_URL, loginData.value);
    emit('login-success', response.data); 
  } catch (error) {
    console.error("Erro no login:", error);
    if (error.response && error.response.status === 401) {
      loginError.value = "E-mail ou senha inválidos.";
    } else {
      loginError.value = "Erro ao tentar fazer login. Tente novamente.";
    }
  }
}
</script>

<style scoped>
.login-view { display: flex; flex-direction: column; min-height: 100vh; }
.login-content { flex: 1; display: flex; justify-content: center; align-items: center; padding: 40px 20px; }
.login-card {
  width: 100%; max-width: 500px; background-color: var(--color-bg-card); border-radius: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1); position: relative; overflow: hidden; transition: all 0.3s ease;
}
.login-card:hover { transform: translateY(-5px); box-shadow: 0 15px 35px rgba(0, 121, 107, 0.15); }
.card-glow-border {
  position: absolute; top: 0; left: 0; width: 100%; height: 100%; border-radius: 16px;
  background: linear-gradient(135deg, var(--color-green-neon), var(--color-green-medium));
  opacity: 0; transition: opacity 0.4s ease; z-index: 0; animation: spin 4s linear infinite;
}
.login-card:hover .card-glow-border { opacity: 1; }
.card-content { position: relative; z-index: 1; background-color: var(--color-bg-card); border-radius: 14px; padding: 30px 40px; }
@keyframes spin { 100% { transform: rotate(360deg); } }

.toggle-buttons {
  display: flex; justify-content: center; margin-bottom: 30px;
  background-color: var(--color-bg-1); border-radius: 8px; padding: 5px;
}
.toggle-buttons button {
  flex: 1; padding: 10px 15px; border: none; background-color: transparent;
  color: var(--color-green-dark); font-weight: 700; font-size: 1rem; border-radius: 6px;
  cursor: pointer; transition: all 0.3s ease;
}

.toggle-buttons button:focus,
.toggle-buttons button:focus-visible {
  outline: none;
}

.toggle-buttons button:focus-visible {
  box-shadow: 0 0 0 3px var(--color-bg-1), 0 0 0 5px var(--color-green-medium);
}

.toggle-buttons button.active {
  background-color: var(--color-green-medium); color: var(--color-text-light);
  box-shadow: 0 4px 10px rgba(0, 173, 154, 0.3);
}

.toggle-buttons button.active:focus-visible {
  box-shadow: 0 0 0 3px var(--color-bg-1), 0 0 0 5px var(--color-green-dark);
}

.auth-form { width: 100%; }
.form-title {
  font-family: 'Exo 2', sans-serif; color: var(--color-green-dark); font-size: 1.8rem;
  font-weight: 700; margin: 0 0 25px 0; text-align: center;
}
.form-group { margin-bottom: 20px; }
.form-group label { display: block; margin-bottom: 8px; font-weight: 700; color: var(--color-green-dark); font-size: 0.9rem; }
.form-input {
  width: 100%; padding: 12px 15px; border: 1px solid #dcdcdc; border-radius: 8px;
  font-family: 'Poppins', sans-serif; font-size: 1rem; background-color: #f7f9f8; color: var(--color-text-dark); transition: all 0.3s ease;
}
.form-input:focus { outline: none; border-color: var(--color-green-medium); box-shadow: 0 0 10px rgba(0, 173, 154, 0.3); background-color: #ffffff; }
.form-button {
  width: 100%; padding: 15px; border: none; border-radius: 8px;
  background: linear-gradient(90deg, var(--color-green-dark) 0%, var(--color-green-medium) 100%);
  color: var(--color-text-light); font-size: 1.1rem; font-weight: 700; cursor: pointer; transition: all 0.3s ease; margin-top: 10px;
}
.form-button:hover { transform: translateY(-3px); box-shadow: 0 10px 20px rgba(0, 121, 107, 0.3); }

.feedback-message { padding: 10px; border-radius: 8px; margin-top: 20px; text-align: center; font-weight: 700; font-size: 0.9rem; }
.error { background-color: #fde8e8; color: #c94a4a; border: 1px solid #c94a4a; }
</style>