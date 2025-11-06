<template>
  <div class="search-card">
    <div class="card-glow-border"></div>
    <div class="card-content">
      
      <h1 class="search-title">Calendário da Coleta Seletiva</h1>
      <p class="search-subtitle">Digite o nome do seu bairro para consultar os dias e horários da coleta.</p>

      <form class="search-form" @submit.prevent="handleSearch" @input="resetSearchState">
        <input 
          type="text" 
          class="search-input" 
          placeholder="Digite o nome do seu bairro (ex: Zona 07)"
          v-model="bairroQuery"
          required
        >
        <button type="submit" class="search-button">Buscar</button>
      </form>

      <div class="results-container">
        
        <div v-if="isLoading" class="loading-spinner"></div>
        
        <div v-if="!isLoading && searchPerformed && results.length === 0" class="results-message">
          Nenhum calendário encontrado para "{{ bairroQuery }}". Verifique o nome do bairro.
        </div>

        <ul v-if="!isLoading && results.length > 0" class="results-list">
          <li v-for="item in results" :key="item.id" class="result-item">
            <span :class="['coleta-tipo', getTipoColetaClass(item.tipoColeta)]">
              {{ item.tipoColeta }}
            </span>
            <span class="coleta-dias">
              <strong>Dias:</strong> {{ item.diasSemana }}
            </span>
            <span class="coleta-horario">
              <strong>Horário:</strong> {{ item.horario }}
            </span>
          </li>
        </ul>

      </div>

    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const API_CALENDARIO_URL = 'http://localhost:8080/api/calendarios/buscar';

const bairroQuery = ref("");
const results = ref([]);
const isLoading = ref(false);
const searchPerformed = ref(false);

function resetSearchState() {
  searchPerformed.value = false;
  results.value = [];
}

async function handleSearch() {
  isLoading.value = true;
  searchPerformed.value = true;
  results.value = [];

  try {
    const response = await axios.get(API_CALENDARIO_URL, {
      params: {
        bairro: bairroQuery.value
      }
    });
    results.value = response.data;
  } catch (error) {
    console.error("Erro ao buscar calendário:", error);
  } finally {
    isLoading.value = false;
  }
}

function getTipoColetaClass(tipo) {
  if (tipo.toLowerCase().includes('reciclável')) {
    return 'tipo-reciclavel';
  }
  if (tipo.toLowerCase().includes('comum')) {
    return 'tipo-comum';
  }
  return '';
}
</script>

<style scoped>
.search-card {
  background-color: var(--color-bg-card);
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
  padding: 30px;
  text-align: center;
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
  margin-top: 30px;
}

.search-card:hover {
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
.search-card:hover .card-glow-border { opacity: 1; }
.card-content {
  position: relative; z-index: 1;
  background-color: var(--color-bg-card); 
  border-radius: 14px;
  padding: 20px;
}
@keyframes spin { 100% { transform: rotate(360deg); } }

.search-title {
  font-family: 'Exo 2', sans-serif;
  color: var(--color-green-dark);
  font-size: 2.5rem;
  font-weight: 800;
  margin: 0 0 10px 0;
}
.search-subtitle {
  color: #555;
  font-size: 1.1rem;
  font-weight: 300;
  margin: 0 0 30px 0;
}

.search-form {
  display: flex;
  gap: 10px;
  margin-bottom: 30px;
}
.search-input {
  flex: 1; 
  padding: 12px 15px;
  border: 1px solid #dcdcdc;
  border-radius: 8px;
  font-family: 'Poppins', sans-serif;
  font-size: 1rem;
  background-color: #f7f9f8;
  color: var(--color-text-dark);
  transition: all 0.3s ease;
}
.search-input:focus {
  outline: none;
  border-color: var(--color-green-medium);
  box-shadow: 0 0 10px rgba(0, 173, 154, 0.3);
  background-color: #ffffff;
}
.search-button {
  padding: 12px 20px;
  border: none;
  border-radius: 8px;
  background: var(--color-green-dark);
  color: var(--color-text-light);
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
}
.search-button:hover {
  background: var(--color-green-medium);
  box-shadow: 0 5px 15px rgba(0, 121, 107, 0.3);
}

.results-container {
  min-height: 100px;
  text-align: left;
}
.results-message {
  color: #777;
  font-style: italic;
  text-align: center;
  padding-top: 20px;
}
.results-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 15px;
}
.result-item {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 10px 20px;
  padding: 15px;
  background-color: #f7f9f8;
  border-radius: 8px;
  border-left: 5px solid var(--color-green-medium);
}
.coleta-tipo {
  font-weight: 700;
  padding: 5px 10px;
  border-radius: 5px;
  color: var(--color-text-light);
  font-size: 0.9rem;
}
.tipo-reciclavel {
  background-color: var(--color-green-dark);
}
.tipo-comum {
  background-color: #666;
}
.coleta-dias, .coleta-horario {
  color: #333;
}
.coleta-dias strong, .coleta-horario strong {
  color: #000;
}

.loading-spinner {
  display: block;
  margin: 40px auto;
  width: 50px;
  height: 50px;
  border: 5px solid rgba(0, 121, 107, 0.2);
  border-top-color: var(--color-green-dark);
  border-radius: 50%;
  animation: spin 1s linear infinite;
}
</style>