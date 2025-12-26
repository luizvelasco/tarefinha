<script setup>
import { ref } from 'vue';

const emit = defineEmits(['add-task']);

const task = ref('');

const addTask = () => {
    if(!task.value.trim()) return;  
    emit('add-task', task.value.trim());    
    task.value = '';
};
</script>

<template>
    <div class="input-group mb-3">
        <input
            v-model="task"                     
            type="text"
            placeholder="Adicionar uma nova tarefa..."
            class="form-control"
            @keyup.enter="addTask"                
        >
        <button
            @click="addTask"                      
            class="btn btn-success"
        >
            Adicionar
        </button>
    </div>
</template>