<script setup>
// Importa a função ref, usada para criar estados reativos no Vue 3
import { ref, computed } from 'vue';
import TaskStats from './components/TaskStats.vue';
import TaskInput from './components/TaskInput.vue';
import TaskFilter from './components/TaskFilter.vue';
import TaskList from './components/TaskList.vue';

/**
 * Lista reativa de tarefas
 * Cada tarefa será um objeto com:
 * - name: nome da tarefa
 * - completed: se está concluída ou não
 * - state: estado visual ('show' ou 'edit')
 */
const tasks = ref([]);

/**
 * Estados para filtros de busca e status
 */
const filterSearch = ref('');

/**
 * Estado do filtro de status:  
 * "" (todas), "pending" (pendentes), "completed" (concluídas)
 */
const filterStatus = ref('');

const filteredTasks = computed(() => {
  let output = tasks.value;

  if(filterSearch.value) {
    const search = filterSearch.value.toLowerCase();
    output = output.filter(o => o.name.toLowerCase().includes(search));
  }

  if(filterStatus.value === 'pending') {
    return output.filter(o => !o.completed);
  } else if(filterStatus.value === 'completed') {
    output = output.filter(o => o.completed);
  }

  return output;
}); 

const onSearch = (search) => {
  filterSearch.value = search;
};

const onStatus = (status) => {
  filterStatus.value = status;
};

const clearFilters = () => {
  filterSearch.value = '';
  filterStatus.value = '';  
};

/**
 * Adiciona uma nova tarefa à lista
 */
const addTask = (task) => {
  // Impede adicionar tarefa vazia ou só com espaços
  if (!task) { return };

  // Adiciona a nova tarefa ao array
  tasks.value.push({
    id: Date.now(),        // ID único baseado no timestamp
    name: task.trim(), // Nome sem espaços extras
    completed: false,           // Nova tarefa começa como não concluída
    state: 'show'               // Estado inicial é visualização
  });

};

const deleteTask = (task) => {
  const index = tasks.value.findIndex(o => o.id === task.id);
  if (index > -1) {
    tasks.value.splice(index, 1);
  }
};  

const emptyStateMessage = computed(() => {
  let output = 'Nenhuma tarefa cadastrada';
  if(filterSearch.search || filterStatus.value) {
    return 'Nenhuma tarefa encontrada com os filtros aplicados';
  }
  return output
});

</script>

<template>
  <div class="container" style="max-width: 800px;">
    
    <!-- Título da aplicação -->
    <h1 class="text-center my-4">Tarefinha</h1>

    <!-- Stats --> 
    <TaskStats :tasks="tasks" />

    <!-- Add New Task -->
    <TaskInput @add-task="addTask" />

    <!-- Filters --> 
    <TaskFilter 
      v-if="tasks.length"
      @search="onSearch"
      @status="onStatus"
    />  

    <!-- Tasks -->
    <TaskList 
      :tasks="filteredTasks"
      @delete="deleteTask" 
    />

    <!-- Empty state --> 
    <div
      v-if="!filteredTasks.length"
      class="card bg-light"
    > 
      <div class="card-body text-center py-5"> 
        <p class="text-muted mb-0">
          {{ emptyStateMessage }}
        </p> 
      </div> 
    </div>
  </div>
</template>
