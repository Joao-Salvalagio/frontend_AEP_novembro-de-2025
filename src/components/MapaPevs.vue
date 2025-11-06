<template>
  <div class="map-card">
    <div class="card-glow-border"></div>
    <div class="card-content">
      <h1 class="map-title">Explore os Pontos de Coleta</h1>
      <p class="map-subtitle">Encontre o Ponto de Entrega Voluntária (PEV) mais próximo de você.</p>
      
      <div id="map-wrapper">
        <div id="map"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, nextTick } from 'vue';
import axios from 'axios';
import 'leaflet/dist/leaflet.css';
import L from 'leaflet';

const apiURL = 'http://localhost:8080/api/pontos-entrega';
let map = null;

async function buscarPontosDeEntrega() {
  try {
    const response = await axios.get(apiURL);
    return response.data;
  } catch (error) {
    console.error('Erro ao buscar pontos de entrega:', error);
    return [];
  }
}

function inicializarMapa() {
  const maringaCoords = [-23.4253, -51.9386]; 
  
  map = L.map('map', { attributionControl: false }).setView(maringaCoords, 13);

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {}).addTo(map);

  L.control.attribution({
    position: 'bottomright',
    prefix: ''
  }).addAttribution('&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a>').addTo(map);

  nextTick(() => {
    map.invalidateSize();
  });
}

function adicionarMarcadores(pontos) {
  if (!map) return; 

  const neonMarkerIcon = L.divIcon({
    className: 'custom-div-icon', 
    html: "<div class='marker-pin'></div><div class='marker-pulse'></div>",
    iconSize: [30, 42],
    iconAnchor: [15, 42]
  });

  pontos.forEach(ponto => {
    if (ponto.latitude && ponto.longitude) {
      const coords = [ponto.latitude, ponto.longitude];
      
      const marker = L.marker(coords, { icon: neonMarkerIcon }).addTo(map);
      
      const popupContent = `
        <div class="popup-content">
          <div class="popup-title">${ponto.nome}</div>
          <div class="popup-address">${ponto.endereco}</div>
          <hr>
          <div class="popup-accepted">
            <strong>Aceita:</strong> ${ponto.tiposAceitos}
          </div>
        </div>
      `;
      marker.bindPopup(popupContent);
    }
  });
}

onMounted(async () => {
  inicializarMapa();
  const pontos = await buscarPontosDeEntrega();
  adicionarMarcadores(pontos);
});

</script>

<style scoped>
.map-card {
  background-color: var(--color-bg-card);
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
  padding: 30px;
  text-align: center;
  position: relative; 
  overflow: hidden; 
  
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.map-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 40px rgba(0, 121, 107, 0.2);
}

.card-glow-border {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  border-radius: 16px;
  
  background: linear-gradient(135deg, var(--color-green-neon), var(--color-green-medium));
  
  opacity: 0;
  transition: opacity 0.4s ease;
  z-index: 0;
  
  animation: spin 4s linear infinite;
}

.map-card:hover .card-glow-border {
  opacity: 1;
}

.card-content {
  position: relative;
  z-index: 1;
  background-color: var(--color-bg-card); 
  border-radius: 14px;
  padding: 20px;
}

@keyframes spin {
  100% { transform: rotate(360deg); }
}

.map-title {
  font-family: 'Exo 2', sans-serif;
  color: var(--color-green-dark);
  font-size: 2.5rem;
  font-weight: 800;
  margin: 0 0 10px 0;
}

.map-subtitle {
  color: #555;
  font-size: 1.1rem;
  font-weight: 300;
  margin: 0 0 25px 0;
}

#map-wrapper {
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  z-index: 1;
}

#map {
  height: 550px;
  width: 100%;
}

:deep(.leaflet-control-attribution) {
  background: rgba(255, 255, 255, 0.8) !important;
  backdrop-filter: blur(2px);
  padding: 3px 8px !important;
  border-radius: 6px !important;
  font-size: 0.75rem !important;
}
:deep(.leaflet-control-attribution a) {
  color: var(--color-green-dark) !important;
}

:deep(.leaflet-popup-content-wrapper) {
  border-radius: 10px !important;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15) !important;
  border: 1px solid var(--color-green-medium);
  overflow: hidden;
}

:deep(.leaflet-popup-content) {
  margin: 0 !important;
  padding: 0 !important;
  font-family: 'Poppins', sans-serif !important;
  width: 250px !important;
}

:deep(.leaflet-popup-tip) {
  background: var(--color-green-medium) !important;
}
</style>

<style>
.popup-content {
  padding: 0;
  overflow: hidden;
}
.popup-title {
  font-size: 1.1rem;
  font-weight: 700;
  color: #ffffff;
  background-color: var(--color-green-medium);
  padding: 10px 15px;
}
.popup-address {
  font-size: 0.9rem;
  color: #555;
  padding: 10px 15px;
  border-bottom: 1px solid #eee;
}
.popup-accepted {
  font-size: 0.9rem;
  padding: 10px 15px;
}
.popup-accepted strong {
  color: var(--color-green-dark);
}

.custom-div-icon {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}
.marker-pin {
  width: 30px;
  height: 30px;
  border-radius: 50% 50% 50% 0;
  background: var(--color-green-neon);
  position: absolute;
  transform: rotate(-45deg);
  border: 1px solid #000;
  box-shadow: 0 0 10px var(--color-green-neon);
}
.marker-pin::after {
  content: '';
  width: 14px;
  height: 14px;
  margin: 8px 0 0 8px;
  background: var(--color-green-dark);
  position: absolute;
  border-radius: 50%;
}
.marker-pulse {
  background: var(--color-green-neon);
  border-radius: 50%;
  height: 15px;
  width: 15px;
  position: absolute;
  top: 8px;
  left: 8px;
  box-shadow: 0 0 10px var(--color-green-neon);
  opacity: 0.7;
  
  animation: pulse-animation 2s infinite ease-out;
}

@keyframes pulse-animation {
  0% { transform: scale(0.1); opacity: 0.7; }
  70% { transform: scale(2.5); opacity: 0; }
  100% { transform: scale(0.1); opacity: 0; }
}
</style>