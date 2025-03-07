<template>
  <div class="tech-keywords">
    <div
      v-for="(keyword, index) in keywords"
      :key="index"
      class="tech-keyword"
      :style="{ '--rotation': `${getRandomRotation(index)}deg`, '--x': `${getRandomX(index)}%`, '--y': `${getRandomY(index)}%` }"
      :ref="el => { keywordRefs[index] = el }"
    >
      {{ keyword }}
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const keywords = [
  'JavaScript',
  'Python',
  'Linux',
  'Vue',
  'Redis',
  'CSS',
  'HTML',
  'CSS',
  'Docker',
  'MySQL',
  'Go',
  'C/C++'
];

const mousePosition = ref({ x: 0, y: 0 });
const keywordRefs = ref([]);

const getRandomRotation = (index) => {
  return ((index * 23) % 30) - 15;
};

const getRandomX = (index) => {
  // 将页面水平分成3列
  const column = index % 3;
  const baseX = (column * 33) + 5; // 基础位置
  return baseX + (Math.random() * 20); // 添加随机偏移
};

const getRandomY = (index) => {
  // 将页面垂直分成4行
  const row = Math.floor(index / 3);
  const baseY = (row * 25) + 5; // 基础位置
  return baseY + (Math.random() * 15); // 添加随机偏移
};

const handleMouseMove = (event) => {
  mousePosition.value = {
    x: event.clientX,
    y: event.clientY
  };

  keywordRefs.value.forEach((keyword, index) => {
    if (!keyword) return;
    
    const rect = keyword.getBoundingClientRect();
    const keywordCenterX = rect.left + rect.width / 2;
    const keywordCenterY = rect.top + rect.height / 2;
    
    const deltaX = mousePosition.value.x - keywordCenterX;
    const deltaY = mousePosition.value.y - keywordCenterY;
    const distance = Math.sqrt(deltaX * deltaX + deltaY * deltaY);
    
    // 设置影响范围和最大移动距离
    const maxDistance = 300;
    const maxMove = 30;
    
    if (distance < maxDistance) {
      const factor = (1 - distance / maxDistance) * maxMove;
      const moveX = (deltaX / distance) * factor;
      const moveY = (deltaY / distance) * factor;
      
      keyword.style.transform = `translate(${moveX}px, ${moveY}px) rotate(${getRandomRotation(index)}deg)`;
    } else {
      keyword.style.transform = `translate(0, 0) rotate(${getRandomRotation(index)}deg)`;
    }
  });
};

onMounted(() => {
  window.addEventListener('mousemove', handleMouseMove);
});

onUnmounted(() => {
  window.removeEventListener('mousemove', handleMouseMove);
});
</script>

<style scoped>
.tech-keywords {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  pointer-events: none;
  overflow: hidden;
  z-index: -1;
}

.tech-keyword {
  position: absolute;
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-weight: 500;
  color: #1e3b8a49;
  opacity: 0.3;
  transition: transform 0.3s ease;
  cursor: pointer;
  left: var(--x);
  top: var(--y);
  animation: float 15s ease-in-out infinite;
  pointer-events: auto;
  will-change: transform;
}

.tech-keyword:hover {
  transform: translateY(-5px) rotate(0deg) !important;
  color: var(--primary-light);
  background: rgba(30, 58, 138, 0.1);
  opacity: 1;
}

@keyframes float {
  0% {
    transform: translate(0, 0) rotate(var(--rotation));
    opacity: 0.3;
  }
  25% {
    transform: translate(40px, -35px) rotate(calc(var(--rotation) + 8deg));
    opacity: 0.7;
  }
  50% {
    transform: translate(-35px, -45px) rotate(calc(var(--rotation) - 5deg));
    opacity: 0.9;
  }
  75% {
    transform: translate(-40px, -25px) rotate(calc(var(--rotation) + 5deg));
    opacity: 0.7;
  }
  100% {
    transform: translate(0, 0) rotate(var(--rotation));
    opacity: 0.3;
  }
}
</style>