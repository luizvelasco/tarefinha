<script setup>
// Importa a função ref, usada para criar estados reativos no Vue 3
import { ref } from 'vue';

/**
 * Estado reativo para armazenar o texto digitado
 * no input de nova tarefa
 */
const newTask = ref('');

/**
 * Lista reativa de tarefas
 * Cada tarefa será um objeto com:
 * - name: nome da tarefa
 * - completed: se está concluída ou não
 * - state: estado visual ('show' ou 'edit')
 */
const tasks = ref([]);

/**
 * Adiciona uma nova tarefa à lista
 */
const addTask = () => {
  // Impede adicionar tarefa vazia ou só com espaços
  if (!newTask.value.trim()) return;

  // Adiciona a nova tarefa ao array
  tasks.value.push({
    name: newTask.value.trim(), // Nome sem espaços extras
    completed: false,           // Nova tarefa começa como não concluída
    state: 'show'               // Estado inicial é visualização
  });

  // Limpa o campo de input após adicionar
  newTask.value = '';
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
</script>

<template>
  <div class="container" style="max-width: 800px;">
    
    <!-- Título da aplicação -->
    <h1 class="text-center my-4">Tarefinha</h1>

    <!-- Campo para adicionar nova tarefa -->
    <div class="input-group mb-3">
      <input
        v-model="newTask"                     
        type="text"
        placeholder="Adicionar uma nova tarefa..."
        class="form-control"
        @keyup.enter="addTask"                
      >
      <button
        @click="addTask"                      
        class="btn btn-success">
        Adicionar
      </button>
    </div>

    <!-- Debug: mostra o array de tarefas em tempo real -->
    <pre>{{ tasks }}</pre>

    <!-- Lista de tarefas -->
    <ul class="list-group">
      <li
        v-for="task in tasks"                 
        :key="task.name"                      
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
          <button class="btn btn-danger btn-sm">
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

      </li>
    </ul>
  </div>
</template>
