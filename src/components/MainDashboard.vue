<template>
  <div class="dashboard-layout">
    <TheHeader />

    <MenuNavegacao @change-view="setView" :active-view="currentView" />

    <main class="content-wrapper-dashboard">
      
      <MapaPevs v-if="currentView === 'mapa' || currentView === 'todos'" />
      <CalendarioBusca v-if="currentView === 'calendario' || currentView === 'todos'" />
      <FormAgendamento 
        v-if="currentView === 'agendamento' || currentView === 'todos'" 
        :user-id="user?.id" 
      /> 

      <div v-if="currentView === 'inicio'" class="welcome-message-dashboard">
        <h2 class="welcome-title">Bem-vindo(a), {{ user?.nome }}!</h2> 
        <p class="welcome-subtitle">Selecione uma das opções acima para começar a interagir.</p>
        
        <button @click="$emit('logout')" class="logout-button logout-button-red">Sair</button> 
      
      </div>

    </main>
    
    <TheFooter />
  </div>
</template>

<script setup>
import { ref } from 'vue'; 
import TheHeader from './TheHeader.vue';
import MenuNavegacao from './MenuNavegacao.vue'; 
import MapaPevs from './MapaPevs.vue';
import CalendarioBusca from './CalendarioBusca.vue';
import FormAgendamento from './FormAgendamento.vue'; 
import TheFooter from './TheFooter.vue';

const props = defineProps({ user: Object });
const emit = defineEmits(['logout']);
const currentView = ref('inicio'); 
function setView(viewName) { currentView.value = viewName; }
</script>

<style scoped>
.dashboard-layout { display: flex; flex-direction: column; min-height: 100vh; }
.content-wrapper-dashboard { flex: 1; width: 100%; max-width: 1200px; margin: 0 auto; padding: 30px 20px; }
.welcome-message-dashboard {
  text-align: center; padding: 60px 20px; background-color: var(--color-bg-card);
  border-radius: 16px; box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08); margin-top: 30px;
}
.welcome-title {
  font-family: 'Exo 2', sans-serif; color: var(--color-green-dark); font-size: 2.2rem;
  font-weight: 800; margin: 0 0 10px 0;
}
.welcome-subtitle { font-size: 1.1rem; color: #555; font-weight: 300; margin: 0 0 30px 0; }

.logout-button {
  padding: 10px 20px; font-family: 'Poppins', sans-serif; font-weight: 600; font-size: 0.9rem;
  border: 2px solid #aaa; background-color: transparent; color: #777;
  border-radius: 8px; cursor: pointer; transition: all 0.3s ease;
}
.logout-button:hover { background-color: #eee; color: #333; border-color: #888; }

.logout-button-red {
  border-color: #E57373;
  color: #D32F2F;
}
.logout-button-red:hover {
  background-color: #D32F2F;
  color: var(--color-text-light);
  border-color: #D32F2F;
  box-shadow: 0 5px 15px rgba(211, 47, 47, 0.3);
}
</style>