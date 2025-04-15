<template>
    <div class="kanban-wrapper">
      <BaseModal v-if="showModal" @close="showModal = false">
        <NewTaskForm
          @task-added="addTask"
          @cancel="showModal = false"
        />
      </BaseModal>

      <BaseModal v-if="showEditModal" @close="closeEditModal">
        <EditTaskModal
            :task="taskToEdit"
            @updated="loadTasks"
            @cancel="closeEditModal"
        />
      </BaseModal>
  
      <div class="kanban-board">
        <KanbanColumn
          v-for="status in statuses"
          :key="status"
          :title="statusLabels[status]"
          :tasks="filteredTasks(status)"
          @drop-task="handleDrop"
          @delete-task="deleteTask"
          @edit-task="handleEditTask"
        />
        

      </div>
  
      <button class="floating-btn" @click="showModal = true">+</button>
    </div>
  </template>
  
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import KanbanColumn from './KanbanColumn.vue'
  import NewTaskForm from './NewTaskForm.vue'
  import BaseModal from './BaseModal.vue'
  import EditTaskModal from './EditTaskModal.vue'

  import {
    getTasks,
    updateTask,
    deleteTask as deleteTaskAPI
  } from '../services/taskService'
  
  const tasks = ref([])
  const showForm = ref(false)

  const showModal = ref(false)

  const taskToEdit = ref(null)
  const showEditModal = ref(false)

function handleEditTask(task) {
  taskToEdit.value = task
  showEditModal.value = true
}

  
  const statuses = ['TODO', 'DOING', 'DONE']
  const statusLabels = {
    TODO: 'A Fazer',
    DOING: 'Fazendo',
    DONE: 'Feito'
  }
  
  onMounted(async () => {
    const { data } = await getTasks()
    tasks.value = data
  })
  
  function filteredTasks(status) {
    return tasks.value.filter(t => t.status === status.toUpperCase())
  }
  
  async function handleDrop(task, newStatus) {
    if (task.status === newStatus) return
    const updated = { ...task, status: newStatus.toUpperCase() }
    await updateTask(task.id, updated)
  
    const index = tasks.value.findIndex(t => t.id === task.id)
    if (index !== -1) {
      tasks.value[index] = updated
    }
  }
  
  async function deleteTask(id) {
    await deleteTaskAPI(id)
    tasks.value = tasks.value.filter(t => t.id !== id)
  }
  
  async function addTask() {
  await loadTasks()
  showModal.value = false
  }

  async function loadTasks() {
  try {
    const { data } = await getTasks()
    tasks.value = data
  } catch (error) {
    console.error('Erro ao carregar tarefas:', error)
  }
}
function closeEditModal() {
  showEditModal.value = false
  taskToEdit.value = null
}


onMounted(() => {
  loadTasks()
})

  </script>
  
  <style scoped>
  .kanban-wrapper {
    position: relative;
    padding-bottom: 5rem;
  }
  
  .kanban-board {
    display: flex;
    gap: 1rem;
    padding: 2rem;
  }
  
  .floating-btn {
    position: fixed;
  bottom: 16px;
  right: 16px;
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background-color: #4CAF50;
  color: white;
  font-size: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 5px rgba(0,0,0,0.3);
  border: none;
  cursor: pointer;
  }
  </style>
  