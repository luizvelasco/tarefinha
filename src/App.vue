<script setup>
// Importa a função ref, usada para criar estados reativos no Vue 3
import { ref, computed } from 'vue';
import TaskStats from './components/TaskStats.vue';
import TaskInput from './components/TaskInput.vue';
import TaskFilter from './components/TaskFilter.vue';

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

/**
 * Coloca a tarefa em modo de edição
 * Faz backup dos valores originais
 */
const editTask = (task) => {
  // Salva o nome original caso ainda não exista backup
  if (!Object.hasOwn(task, '_name')) {
    task._name = task.name;
  }

  // Salva o estado original de concluído
  if (!Object.hasOwn(task, '_completed')) {
    task._completed = task.completed;
  }

  // Altera o estado para edição
  task.state = 'edit';
};

/**
 * Confirma as alterações feitas na edição
 */
const commitTask = (task) => {
  // Atualiza o nome com o valor editado
  if (Object.hasOwn(task, '_name')) {
    task.name = task._name;
  }

  // Atualiza o status de concluído
  if (Object.hasOwn(task, '_completed')) {
    task.completed = task._completed;
  }

  // Volta para o modo de visualização
  task.state = 'show';
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

    <!-- Lista de tarefas -->
    <ul class="list-group">
      <li
        v-for="task in filteredTasks"                 
        :key="task.id"                      
        class="list-group-item d-flex align-items-center gap-2"
      >

        <!-- MODO VISUALIZAÇÃO -->
        <template v-if="task.state === 'show'">
          <!-- Checkbox de concluído -->
          <input
            type="checkbox"
            v-model="task.completed"
            class="form-check-input"
          >

          <!-- Nome da tarefa -->
          <span
            class="flex-grow-1"
            :class="task.completed
              ? 'text-decoration-line-through text-muted'
              : null"
          >
            {{ task.name }}
          </span>

          <!-- Botão editar -->
          <button
            class="btn btn-primary btn-sm"
            @click="editTask(task)">
            Editar
          </button>

          <!-- Botão excluir (ainda não implementado) -->
          <button class="btn btn-danger btn-sm" @click="task.state = 'delete'">
            Excluir
          </button>
        </template>

        <!-- MODO EDIÇÃO -->
        <template v-else-if="task.state === 'edit'">
          <!-- Checkbox editável -->
          <input
            type="checkbox"
            v-model="task._completed"
            class="form-check-input"
          >

          <!-- Input para editar o nome -->
          <input
            type="text"
            v-model="task._name"
            class="form-control form-control-sm"
          >

          <!-- Salvar alterações -->
          <button
            class="btn btn-success btn-sm"
            @click="commitTask(task)">
            Salvar
          </button>

          <!-- Cancelar edição -->
          <button
            class="btn btn-secondary btn-sm"
            @click="task.state = 'show'">
            Cancelar
          </button>
        </template>
        
        <template v-else-if="task.state === 'delete'">
          <span class="flex-grow-1"> 
            <div class="fw-semibold">{{ task.name }}</div> 
            <div class="text-muted small">Tem certeza que deseja remover?</div> 
          </span> 
          <button class="btn btn-danger btn-sm" @click="deleteTask(task)">Sim, excluir</button> 
          <button class="btn btn-outline-secondary btn-sm" @click="task.state = 'show'">Cancelar</button> 
        </template>
      
      </li>
    </ul>
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
