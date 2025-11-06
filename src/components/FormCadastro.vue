<template>
  <div class="form-card-inner"> <h1 class="form-title">Crie sua Conta</h1>
      <p class="form-subtitle">Cadastre-se para agendar coletas e participar da comunidade.</p>

      <form @submit.prevent="handleSubmit">
        
        <div class="form-group">
          <label for="nome">Nome Completo:</label>
          <input 
            type="text" 
            id="nome" 
            class="form-input" 
            v-model="formData.nome"
            placeholder="Ex: João Miguel Silva" 
            required
          >
        </div>

        <div class="form-group">
          <label for="email">E-mail:</label>
          <input 
            type="email" 
            id="email" 
            class="form-input" 
            v-model="formData.email"
            placeholder="Ex: joao@email.com" 
            required
          >
        </div>

        <div class="form-group">
          <label for="senha">Senha:</label>
          <input 
            type="password" 
            id="senha" 
            class="form-input" 
            v-model="formData.senha"
            placeholder="Mínimo 6 caracteres" 
            required
          >
        </div>

        <div class="form-group">
          <label for="bairro">Seu Bairro:</label>
          <input 
            type="text" 
            id="bairro" 
            class="form-input" 
            v-model="formData.bairro"
            placeholder="Ex: Zona 07" 
            required
          >
        </div>

        <div class="form-group">
          <button type="submit" class="form-button">
            Criar Conta
          </button>
        </div>

        <div v-if="successMessage" class="feedback-message success">
          {{ successMessage }}
        </div>
        <div v-if="errorMessage" class="feedback-message error">
          {{ errorMessage }}
        </div>

      </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const API_CADASTRO_URL = 'http://localhost:8080/api/usuarios';

const formData = ref({
  nome: "",
  email: "",
  senha: "",
  bairro: ""
});

const successMessage = ref("");
const errorMessage = ref("");

async function handleSubmit() {
  successMessage.value = "";
  errorMessage.value = "";

  const cadastroDTO = {
    nome: formData.value.nome,
    email: formData.value.email,
    senha: formData.value.senha,
    bairro: formData.value.bairro
  };

  try {
    const response = await axios.post(API_CADASTRO_URL, cadastroDTO);
    successMessage.value = `Usuário "${response.data.nome}" cadastrado com sucesso! Faça o login.`; // Mensagem de sucesso
    formData.value = { nome: "", email: "", senha: "", bairro: "" }; // Limpa form
  } catch (error) {
    console.error("Erro ao cadastrar usuário:", error);
    if (error.response && error.response.status === 400) {
      errorMessage.value = error.response.data;
    } else {
      errorMessage.value = "Falha ao cadastrar. Tente novamente.";
    }
  }
}
</script>

<style scoped>
.form-card-inner {
  width: 100%;
}

.form-title {
  font-family: 'Exo 2', sans-serif;
  color: var(--color-green-dark);
  font-size: 1.8rem;
  font-weight: 700;
  margin: 0 0 25px 0;
  text-align: center;
}
.form-subtitle {
  color: #555;
  font-size: 1rem;
  font-weight: 300;
  margin: 0 0 30px 0;
  text-align: center;
}

.form-group {
  margin-bottom: 20px;
}
.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 700;
  color: var(--color-green-dark);
  font-size: 0.9rem;
}

.form-input {
  width: 100%;
  padding: 12px 15px;
  border: 1px solid #dcdcdc;
  border-radius: 8px;
  font-family: 'Poppins', sans-serif;
  font-size: 1rem;
  background-color: #f7f9f8;
  color: var(--color-text-dark);
  transition: all 0.3s ease;
}
.form-input::placeholder { color: #999; }
.form-input:focus {
  outline: none;
  border-color: var(--color-green-medium);
  box-shadow: 0 0 10px rgba(0, 173, 154, 0.3);
  background-color: #ffffff;
}

.form-button {
  width: 100%;
  padding: 15px;
  border: none;
  border-radius: 8px;
  background: linear-gradient(90deg, var(--color-green-dark) 0%, var(--color-green-medium) 100%);
  color: var(--color-text-light);
  font-size: 1.1rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 10px; 
}
.form-button:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 20px rgba(0, 121, 107, 0.3);
}

.feedback-message {
  padding: 10px;
  border-radius: 8px;
  margin-top: 20px;
  text-align: center;
  font-weight: 700;
  font-size: 0.9rem;
}
.success {
  background-color: #E6FFFA;
  color: var(--color-green-dark);
  border: 1px solid var(--color-green-medium);
}
.error {
  background-color: #fde8e8;
  color: #c94a4a;
  border: 1px solid #c94a4a;
}
</style>