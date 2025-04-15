<template>
    <form @submit.prevent="submitUpdate" class="edit-task-form">
      <h3>Editar Tarefa</h3>
  
      <input v-model="form.title" placeholder="Título" required />
      <textarea v-model="form.description" placeholder="Descrição" required />
      <input type="date" v-model="form.expire" required />
  
      <select v-model="form.status" required>
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
  import { reactive, watch } from 'vue'
  import { updateTask } from '../services/taskService'
  
  const props = defineProps({ task: Object })
  const emit = defineEmits(['updated', 'cancel'])
  
  const form = reactive({ ...props.task })
  
  watch(() => props.task, (newTask) => {
    Object.assign(form, newTask)
  })
  
  async function submitUpdate() {
    try {
      await updateTask(form.id, form)
      emit('updated')
      emit('cancel')
    } catch (err) {
      console.error('Erro ao atualizar task:', err)
    }
  }
  </script>
  
  <style scoped>
  .edit-task-form {
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
  