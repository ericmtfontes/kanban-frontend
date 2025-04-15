<template>
    <div class="modal-overlay" @click.self="$emit('close')">
      <div class="modal-content">
        <slot />
      </div>
    </div>
  </template>
  
  <script setup>
  defineEmits(['close'])
  </script>
  
  <style scoped>
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }
  
  .modal-content {
    background-color: #fff;
    padding: 2rem;
    border-radius: 10px;
    max-width: 500px;
    width: 90%;
  }
  </style>
  