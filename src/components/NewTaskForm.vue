<template>
    <form @submit.prevent="submitForm" class="task-form">
      <h3>Nova Tarefa</h3>
  
      <input v-model="title" placeholder="Título" required />
      <textarea v-model="description" placeholder="Descrição" required />
      <input type="date" v-model="expire" required />
  
      <select v-model="status" required>
        <option value="TODO">A Fazer</option>
        <option value="DOING">Fazendo</option>
        <option value="DONE">Feito</option>
      </select>
  
      <div class="actions">
        <button type="submit">Salvar</button>
        <button type="button" @click="$emit('cancel')">Cancelar</button>
      </div>
    </form>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  import { createTask } from '../services/taskService'
  
  const emit = defineEmits(['task-added', 'cancel'])
  
  const title = ref('')
  const description = ref('')
  const expire = ref('')
  const status = ref('TODO')
  
  async function submitForm() {
    const newTask = {
      title: title.value,
      description: description.value,
      expire: expire.value,
      status: status.value
    }
  
    try {
      const { data } = await createTask(newTask)
      emit('task-added', data)
      resetForm()
    } catch (error) {
      alert('Erro ao criar tarefa.')
    }
  }
  
  function resetForm() {
    title.value = ''
    description.value = ''
    expire.value = ''
    status.value = 'TODO'
    emit('cancel')
  }
  </script>
  
  
  <style scoped>
   .task-form {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }
  .actions {
    display: flex;
    justify-content: flex-end;
    gap: 0.5rem;
  }
  </style>
  