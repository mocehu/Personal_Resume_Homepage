<template>
  <div v-if="show" class="qrcode-modal" @click="closeModal">
    <div class="modal-content" @click.stop>
      <h3 class="modal-title">{{ title }}</h3>
      <img :src="qrcodeUrl" :alt="title" class="qrcode-image">
      <button class="close-button" @click="closeModal">
        <i class="fas fa-times"></i>
      </button>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  show: {
    type: Boolean,
    required: true
  },
  title: {
    type: String,
    required: true
  },
  qrcodeUrl: {
    type: String,
    required: true
  }
});

const emit = defineEmits(['update:show']);

const closeModal = () => {
  emit('update:show', false);
};
</script>

<style scoped>
.qrcode-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  position: relative;
  max-width: 90%;
  width: 300px;
  text-align: center;
}

.modal-title {
  color: #2c3e50;
  margin-bottom: 1.5rem;
  font-size: 1.2rem;
}

.qrcode-image {
  width: 200px;
  height: 200px;
  object-fit: contain;
}

.close-button {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  background: none;
  border: none;
  font-size: 1.2rem;
  color: #64748b;
  cursor: pointer;
  padding: 0.5rem;
  transition: color 0.2s ease;
}

.close-button:hover {
  color: var(--primary-light);
}
</style>