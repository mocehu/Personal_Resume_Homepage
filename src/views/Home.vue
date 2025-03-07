<template>
  <div class="home">
    <div class="hero">
      <div class="content">
        <h1 class="title">你好，我是<span class="highlight">{{ displayedName }}</span></h1>
        <p class="subtitle">{{ personalInfo.position }}</p>
        <div class="cta-buttons">
          <router-link to="/resume" class="btn btn-primary ">个人简历</router-link>
          <router-link to="/projects" class="btn btn-secondary">查看项目</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { personalInfo } from '../data/personalData';

const fullName = personalInfo.name;
const displayedName = ref('');

const typeText = () => {
  let currentIndex = 0;
  displayedName.value = '';
  
  const interval = setInterval(() => {
    if (currentIndex < fullName.length) {
      displayedName.value += fullName[currentIndex];
      currentIndex++;
    } else {
      clearInterval(interval);
      setTimeout(() => {
        typeText();
      }, 2000); // 2秒后重新开始打字
    }
  }, 200);
};

onMounted(() => {
  typeText();
});
</script>

<style scoped>
.home {
  height: 100%;
  display: flex;
  align-items: center;
  width: 100%;
}

.hero {
  width: 100%;
  text-align: center;
}

.content {
  max-width: 800px;
  margin: 0 auto;
  padding: 0 2rem;
  animation: fadeIn 1s ease-out;
}

.title {
  font-size: 3.5rem;
  font-weight: 700;
  margin-bottom: 1rem;
  color: #2c3e50;
}

.highlight {
  background: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.subtitle {
  font-size: 1.5rem;
  color: #64748b;
  margin-bottom: 2rem;
}

.cta-buttons {
  display: flex;
  gap: 1rem;
  justify-content: center;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 768px) {
  .title {
    font-size: 2.5rem;
  }
  
  .subtitle {
    font-size: 1.25rem;
  }
  
  .cta-buttons {
    flex-direction: column;
  }
}
</style>