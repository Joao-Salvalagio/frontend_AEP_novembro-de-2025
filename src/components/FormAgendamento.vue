<template>
  <div class="form-card">
    <div class="card-glow-border"></div>
    <div class="card-content">
      
      <h1 class="form-title">Agendar Coleta de Volumosos</h1>
      <p class="form-subtitle">Preencha os dados abaixo para solicitar o recolhimento de móveis e eletrônicos.</p>

      <form @submit.prevent="handleSubmit">
        
        <div class="form-group">
          <label for="descricao">O que será coletado?</label>
          <textarea 
            id="descricao" 
            class="form-textarea" 
            v-model="descricaoItem"
            placeholder="Ex: Sofá 3 lugares, cor azul" 
            required
          ></textarea>
        </div>

        <div class="form-row">
          
          <div class="form-group half-width">
            <label for="data">Melhor data para coleta:</label>
            <input 
              type="date" 
              id="data" 
              class="form-input" 
              v-model="dataAgendamento" 
              required
            >
          </div>
          
          <div class="form-group half-width">
            <label for="cooperativa">Escolha a cooperativa:</label>
            <select 
              id="cooperativa" 
              class="form-select" 
              v-model="selectedCooperativa" 
              required
            >
              <option value="" disabled>Selecione uma opção</option>
              <option v-for="coop in cooperativasList" :key="coop.id" :value="coop.id">
                {{ coop.nome }} (Coleta: {{ coop.tiposColetados }})
              </option>
            </select>
          </div>

        </div>

        <div class="form-group">
          <button type="submit" class="form-button">
            Solicitar Agendamento
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
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const API_COOPERATIVAS_URL = 'http://localhost:8080/api/cooperativas';
const API_AGENDAMENTOS_URL = 'http://localhost:8080/api/agendamentos';

const cooperativasList = ref([]); 
const selectedCooperativa = ref(""); 
const descricaoItem = ref("");
const dataAgendamento = ref("");

const successMessage = ref("");
const errorMessage = ref("");

onMounted(async () => {
  try {
    const response = await axios.get(API_COOPERATIVAS_URL);
    cooperativasList.value = response.data;
  } catch (error) {
    console.error("Erro ao buscar cooperativas:", error);
    errorMessage.value = "Não foi possível carregar a lista de cooperativas.";
  }
});

async function handleSubmit() {

  successMessage.value = "";
  errorMessage.value = "";

  const usuarioId = 1;

  const agendamentoDTO = {
    descricaoItem: descricaoItem.value,
    dataAgendamento: dataAgendamento.value,
    cooperativaId: selectedCooperativa.value,
    usuarioId: usuarioId 
  };

  try {
    const response = await axios.post(API_AGENDAMENTOS_URL, agendamentoDTO);
    
    successMessage.value = `Agendamento (ID: ${response.data.id}) solicitado com sucesso!`;
    
    descricaoItem.value = "";
    dataAgendamento.value = "";
    selectedCooperativa.value = "";

  } catch (error) {
    console.error("Erro ao criar agendamento:", error);
    errorMessage.value = "Falha ao solicitar agendamento. Tente novamente.";
  }
}
</script>

<style scoped>
.form-card {
  background-color: var(--color-bg-card);
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
  padding: 30px;
  text-align: left; 
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
  margin-top: 30px; 
}

.form-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 40px rgba(0, 121, 107, 0.2);
}
.card-glow-border {
  position: absolute; top: 0; left: 0;
  width: 100%; height: 100%;
  border-radius: 16px;
  background: linear-gradient(135deg, var(--color-green-neon), var(--color-green-medium));
  opacity: 0;
  transition: opacity 0.4s ease;
  z-index: 0;
  animation: spin 4s linear infinite;
}
.form-card:hover .card-glow-border { opacity: 1; }
.card-content {
  position: relative; z-index: 1;
  background-color: var(--color-bg-card); 
  border-radius: 14px;
  padding: 20px;
}
@keyframes spin { 100% { transform: rotate(360deg); } }

.form-title {
  font-family: 'Exo 2', sans-serif;
  color: var(--color-green-dark);
  font-size: 2.5rem;
  font-weight: 800;
  margin: 0 0 10px 0;
  text-align: center;
}
.form-subtitle {
  color: #555;
  font-size: 1.1rem;
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
}

.form-input,
.form-select,
.form-textarea {
  width: 100%;
  padding: 12px 15px;
  border: 1px solid #dcdcdc;
  border-radius: 8px;
  font-family: 'Poppins', sans-serif;
  font-size: 1rem;
  transition: all 0.3s ease;
  
  background-color: #f7f9f8;
  color: var(--color-text-dark);
}

.form-input::placeholder,
.form-textarea::placeholder {
  color: #999;
}

.form-select:required:invalid {
  color: #999;
}
.form-select option {
  color: var(--color-text-dark);
}
.form-select option[value=""][disabled] {
  color: #999;
}

.form-input:focus,
.form-select:focus,
.form-textarea:focus {
  outline: none;
  border-color: var(--color-green-medium);
  box-shadow: 0 0 10px rgba(0, 173, 154, 0.3);
  background-color: #ffffff; 
}
.form-textarea {
  min-height: 100px;
  resize: vertical;
}


.form-row {
  display: flex;
  gap: 20px;
}
.half-width {
  flex: 1; 
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
}
.form-button:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 20px rgba(0, 121, 107, 0.3);
}

.feedback-message {
  padding: 15px;
  border-radius: 8px;
  margin-top: 20px;
  text-align: center;
  font-weight: 700;
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