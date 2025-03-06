<template>
  <div class="not-found">
    <div class="content">
      <h1 class="error-code">404</h1>
      <p class="error-message">抱歉，您访问的页面不存在</p>
      <router-link to="/" class="btn btn-primary">返回首页</router-link>
    </div>
  </div>
</template>

<style scoped>
.not-found {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  background: linear-gradient(135deg, var(--primary-light) 0%, var(--primary-dark) 100%);
  color: var(--primary-dark);
}

.content {
  padding: 2rem;
  animation: fadeIn 1s ease-out;
}

.error-code {
  font-size: 8rem;
  font-weight: 700;
  margin: 0;
  line-height: 1;
  text-shadow: 2px 2px 10px rgba(0, 0, 0, 0.2);
  animation: pulse 2s infinite;
}

.error-message {
  font-size: 1.5rem;
  margin: 1rem 0 2rem;
  opacity: 0.9;
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

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
  }
}

@media (max-width: 768px) {
  .error-code {
    font-size: 6rem;
  }
  
  .error-message {
    font-size: 1.25rem;
  }
}
</style>