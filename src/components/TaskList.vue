<script setup>
const props = defineProps(['tasks'])

const emit = defineEmits(['delete']);

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
  emit('delete', task);
};  

</script>

<template>
    <ul class="list-group">
        <li
        v-for="task in props.tasks"                 
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
</template>