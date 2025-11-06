<template>
  <footer class="main-footer">
    <p>&copy; 2025 - Projeto AEP - Vamos Reciclar Maringá</p>
    <div class="footer-links">
      <a href="#" class="footer-link">Termos de Uso</a>
      <span class="link-separator">|</span>
      <a href="#" class="footer-link">Política de Privacidade</a>
    </div>
  </footer>
</template>

<script setup>
</script>

<style scoped>
.main-footer {
  background: linear-gradient(90deg, var(--color-green-dark) 0%, var(--color-green-medium) 100%);
  color: var(--color-text-light);
  text-align: center;
  padding: 30px 20px;
  margin-top: auto;
  box-shadow: 0 -4px 15px rgba(0, 121, 107, 0.2);
  position: relative;
  z-index: 10;
}

.main-footer p {
  margin: 0 0 10px 0;
  font-weight: 300;
  font-size: 0.9rem;
}

.footer-link {
  color: var(--color-text-light);
  text-decoration: none;
  font-size: 0.9rem;
  font-weight: 400;
  padding: 0 5px;
  opacity: 0.8;
  transition: all 0.3s ease;
}

.footer-link:hover {
  opacity: 1;
  color: var(--color-green-neon);
  text-shadow: 0 0 8px var(--color-green-neon);
}

.link-separator {
  margin: 0 8px;
  opacity: 0.4;
}
</style>