<template>
  <nav class="main-navigation">
    <div class="nav-container">
      
      <button @click="$emit('change-view', 'inicio')" class="nav-button" :class="{ active: activeView === 'inicio' }">
        <span class="icon">🏠</span> Início
      </button>

      <button @click="$emit('change-view', 'mapa')" class="nav-button" :class="{ active: activeView === 'mapa' }">
        <span class="icon">🌍</span> Explorar Pontos de Coleta
      </button>
      
      <button @click="$emit('change-view', 'calendario')" class="nav-button" :class="{ active: activeView === 'calendario' }">
        <span class="icon">📅</span> Calendário da Coleta
      </button>
      
      <button @click="$emit('change-view', 'agendamento')" class="nav-button" :class="{ active: activeView === 'agendamento' }">
        <span class="icon">🚛</span> Agendar Coleta
      </button>
      
      <button @click="$emit('change-view', 'todos')" class="nav-button" :class="{ active: activeView === 'todos' }">
        <span class="icon">🚀</span> Todas as Opções (Site Completo)
      </button>
    </div>
  </nav>
</template>

<script setup>
defineProps({
  activeView: String 
});
defineEmits(['change-view']);
</script>

<style scoped>
.main-navigation {
  width: 100%;
  background: var(--color-bg-card);
  box-shadow: 0 4px 15px rgba(0, 121, 107, 0.1);
  padding: 20px;
  display: flex;
  justify-content: center;
  position: relative; 
  z-index: 900;
}
.nav-container {
  width: 100%;
  max-width: 1200px;
  display: flex;
  justify-content: center;
  gap: 15px;
  flex-wrap: wrap;
}
.nav-button {
  display: flex; align-items: center; gap: 8px;
  padding: 12px 25px;
  font-family: 'Exo 2', sans-serif; font-weight: 600; font-size: 1rem;
  border: 2px solid var(--color-green-medium);
  background-color: transparent; color: var(--color-green-medium);
  border-radius: 8px; cursor: pointer;
  transition: all 0.3s ease;
}
.nav-button:focus, .nav-button:focus-visible { outline: none; }
.nav-button:focus-visible {
  box-shadow: 0 0 0 3px var(--color-bg-1), 0 0 0 5px var(--color-green-medium);
}
.nav-button:hover {
  background-color: var(--color-green-medium); color: var(--color-text-light);
  transform: translateY(-3px) scale(1.03);
  box-shadow: 0 10px 20px rgba(0, 173, 154, 0.2);
}
.nav-button:active { transform: translateY(-1px) scale(1.01); }
.icon { font-size: 1.2rem; }
.nav-button.active {
  background-color: var(--color-bg-1); color: var(--color-green-dark);
  border-color: var(--color-green-medium);
  box-shadow: 0 5px 15px rgba(0, 121, 107, 0.15);
}
.nav-button.active:hover {
  background-color: var(--color-bg-1); color: var(--color-green-dark);
  border-color: var(--color-green-medium);
  transform: translateY(-3px) scale(1.03);
  box-shadow: 0 10px 20px rgba(0, 173, 154, 0.2);
}
.nav-button.active:focus-visible {
   box-shadow: 0 0 0 3px var(--color-bg-1), 0 0 0 5px var(--color-green-dark);
}
</style>