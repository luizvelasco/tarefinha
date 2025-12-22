<script setup>
import { ref } from 'vue';

const newTask = ref('');
const tasks = ref([]);

const addTask = () => {
  if(!newTask.value.trim()) return; // Prevent adding empty tasks
  tasks.value.push(newTask.value);
  newTask.value = '';  // Clear input after adding
}

</script>

<template>
<div class="container" style="max-width: 800px;">
    <h1 class="text-center my-4">Tarefinha</h1>
    
    <!-- Stats -->
    <!-- <div class="card mb-3">
      <div class="card-body">
        <div class="row text-center">
          <div class="col-4">
            <div class="fw-bold fs-4">3</div>
            <div class="text-muted small">Total</div>
          </div>
          <div class="col-4">
            <div class="fw-bold fs-4 text-success">1</div>
            <div class="text-muted small">Concluídas</div>
          </div>
          <div class="col-4">
            <div class="fw-bold fs-4 text-warning">2</div>
            <div class="text-muted small">Pendentes</div>
          </div>
        </div>
      </div>
    </div> -->
    
    <!-- Add new task -->
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

    <pre>{{ tasks }}</pre>

    <!-- Filters -->
    <!-- <div class="d-flex gap-2 mb-3">
      <input type="text" placeholder="Buscar tarefa..." class="form-control" style="flex: 1;">
      <select class="form-select" style="flex: 1;">
        <option value="">Todas</option>
        <option value="pending">Pendentes</option>
        <option value="completed">Concluídas</option>
      </select>
      <button class="btn btn-outline-secondary btn-sm" style="flex-shrink: 0;">Limpar filtros</button>
    </div> -->

    <!-- Tasks -->
    <!-- <ul class="list-group">
      <li class="list-group-item d-flex align-items-center gap-2">
        <input type="checkbox" class="form-check-input">
        <span class="flex-grow-1">Refatorar componente de login</span>
        <button class="btn btn-primary btn-sm">Editar</button>
        <button class="btn btn-danger btn-sm">Excluir</button>
      </li>
      <li class="list-group-item d-flex align-items-center gap-2">
        <input type="checkbox" class="form-check-input" checked>
        <span class="flex-grow-1 text-decoration-line-through text-muted">Escrever testes unitários</span>
        <button class="btn btn-primary btn-sm">Editar</button>
        <button class="btn btn-danger btn-sm">Excluir</button>
      </li>
      <li class="list-group-item d-flex align-items-center gap-2">
        <input type="checkbox" class="form-check-input">
        <input type="text" value="Corrigir bug no modal" class="form-control form-control-sm">
        <button class="btn btn-success btn-sm">Salvar</button>
        <button class="btn btn-secondary btn-sm">Cancelar</button>
      </li>
      <li class="list-group-item d-flex align-items-center gap-2">
        <span class="flex-grow-1">
          <div class="fw-semibold">Atualizar documentação da API</div>
          <div class="text-muted small">Tem certeza que deseja remover?</div>
        </span>
        <button class="btn btn-danger btn-sm">Sim, excluir</button>
        <button class="btn btn-outline-secondary btn-sm">Cancelar</button>
      </li>
    </ul> -->

    <!-- Empty state -->
    <!-- <div class="card bg-light">
      <div class="card-body text-center py-5">
        <p class="text-muted mb-0">Nenhuma tarefa cadastrada</p>
      </div>
    </div> -->
</div>

</template>