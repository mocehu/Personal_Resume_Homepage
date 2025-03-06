<script setup>
import { RouterView, RouterLink } from 'vue-router'
import TechKeywords from './components/TechKeywords.vue'
</script>

<template>
  <div class="app">
    <TechKeywords />

    <nav class="nav">
      <RouterLink to="/" class="nav-link">首页</RouterLink>
      <RouterLink to="/projects" class="nav-link">项目</RouterLink>
      <RouterLink to="/resume" class="nav-link">简历</RouterLink>
    </nav>
    <main class="main">
      <RouterView />
    </main>
  </div>
</template>

<style scoped>
.app {
  height: 100vh;
  display: flex;
  flex-direction: column;
  overflow-x: hidden;
}

.nav {
  background: var(--background-blur);
  backdrop-filter: blur(10px);
  padding: 1rem 2rem;
  display: flex;
  gap: 2rem;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  transition: var(--transition-base);
}

.nav-link {
  color: var(--primary-color);
  text-decoration: none;
  font-weight: 600;
  transition: var(--transition-base);
  position: relative;
  padding: 0.5rem 1rem;
  border-radius: 8px;
}

.nav-link:hover,
.nav-link.router-link-active {
  color: var(--primary-light);
  background: rgba(30, 58, 138, 0.1);
  transform: translateY(-1px);
}

.main {
  padding-top: 4rem;
  flex: 1;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
}
</style>
