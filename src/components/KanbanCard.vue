<template>
    <div
      class="kanban-card"
      draggable="true"
      @dragstart="onDragStart"
    >
      <div class="card-header">
        <h4 class="card-title">{{ task.title }}</h4>
        <button class="edit-btn" @click="$emit('edit-task', task)">✏️</button>
        <button class="delete-btn" @click="$emit('delete-task', task.id)">🗑️</button>
      </div>
  
      <p class="card-description">
        📝 {{ task.description }}
      </p>
  
      <p class="card-expire">
        📅 Expira em: {{ formatDate(task.expire) }}
      </p>
    </div>
  </template>
  
  <script setup>
  const props = defineProps({
    task: {
      type: Object,
      required: true
    }
  })
  
  function onDragStart(event) {
    event.dataTransfer.setData('task', JSON.stringify(props.task))
  }
  
  function formatDate(dateStr) {
    const options = { day: '2-digit', month: '2-digit', year: 'numeric' }
    const localDate = new Date(dateStr + 'T12:00:00')
    return localDate.toLocaleDateString('pt-BR', options)
  }
  </script>
  
  <style scoped>
  .edit-btn {
    background: none;
    border: none;
    color: #ff4444;
    font-size: 1.2rem;
    cursor: pointer;
    padding: 0;
    margin-left: 0.5rem;
}

.edit-btn:hover {
  color: #0056b3;
}
  .kanban-card {
    background-color: #f9f9f9;
    border-left: 5px solid #4caf50;
    border-radius: 8px;
    padding: 1rem;
    margin-bottom: 1rem;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
    transition: transform 0.1s;
    text-align: left; 
  }
  
  .kanban-card:hover {
    transform: scale(1.01);
  }
  
  .card-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
  }
  
  .card-title {
    font-size: 1.1rem;
    font-weight: 600;
    color: #333;
    margin: 0;
    text-align: left;
    flex: 1;
  }
  
  .card-description {
    font-size: 0.95rem;
    margin-top: 0.5rem;
    color: #555;
    text-align: left;
  }
  
  .card-expire {
    font-size: 0.85rem;
    color: #777;
    margin-top: 0.75rem;
    text-align: left;
  }
  
  .delete-btn {
    background: none;
    border: none;
    color: #ff4444;
    font-size: 1.2rem;
    cursor: pointer;
    padding: 0;
    margin-left: 0.5rem;
  }
  
  .delete-btn:hover {
    color: #cc0000;
  }
  </style>
  