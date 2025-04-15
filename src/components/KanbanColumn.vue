<template>
    <div
      class="kanban-column"
      @dragover.prevent
      @drop="onDrop"
    >
    <h3 class="column-title">
      {{ title }}
    </h3>
      <div class="task-list">
        <KanbanCard
          v-for="task in tasks"
          :key="task.id"
          :task="task"
          @delete-task="$emit('delete-task', $event)"
          @dragstart.native="onDragStart"
          @edit-task="$emit('edit-task', $event)"
        />
      </div>
    </div>
  </template>
  
  <script setup>
  import KanbanCard from './KanbanCard.vue'
  
  const props = defineProps({
    title: String,
    tasks: Array
  })
  
  const emit = defineEmits(['drop-task', 'delete-task'])
  
  function onDrop(event) {
    const task = JSON.parse(event.dataTransfer.getData('task'))
    emit('drop-task', task, titleToStatus(props.title))
  }
  
  function titleToStatus(title) {
    const map = {
      'A Fazer': 'TODO',
      'Fazendo': 'DOING',
      'Feito': 'DONE'
    }
    return map[title]
  }
  </script>
  
  <style scoped>
  .kanban-column {
    background-color: #f0f0f0;
    border-radius: 8px;
    padding: 1rem;
    flex: 1;
    min-width: 250px;
  }
  .task-list {
    margin-top: 1rem;
  }
  .column-title {
  padding: 0.5rem;
  border-radius: 6px;
  color: #000; 
  font-weight: bold;
  margin-bottom: 1rem;
  text-align: center;
  background-color: #e0e0e0;
}
  </style>
  