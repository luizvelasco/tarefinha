<script setup>
import { ref, watch } from 'vue';

const emit = defineEmits(['search', 'status']);

const search = ref('');
const status = ref('');

const clearFilters = () => {
  search.value = '';
  status.value = '';
};  

watch(search, (vl) => {
  emit('search', vl);
});

watch(status, (vl) => {
  emit('status', vl);
});

</script>

<template>
    <div class="d-flex gap-2 mb-3"> 
      <input  v-modeltype="text" placeholder="Buscar tarefa..." 
        class="form-control" 
        style="flex: 1;"
        v-model="search"
      > 
      <select class="form-select" style="flex: 1;" v-model="status"> 
        <option value="">Todas</option> 
        <option value="pending">Pendentes</option> 
        <option value="completed">Concluídas</option> 
      </select> 
      <button @click="clearFilters" class="btn btn-outline-secondary btn-sm" style="flex-shrink: 0;">
        Limpar filtros
      </button> 
    </div>
</template>