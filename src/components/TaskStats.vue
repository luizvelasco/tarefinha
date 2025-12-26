<script setup>
import { computed } from 'vue';

const props = defineProps(['tasks']);

const total = computed(() => props.tasks.length);
const completed = computed(() => props.tasks.filter(o => o.completed).length);
const pending = computed(() => props.tasks.filter(o => !o.completed).length);
</script>

<template>
<div class="card mb-3"> 
    <div class="card-body"> 
    <div class="row text-center"> 
        <div class="col-4"> 
        <div class="fw-bold fs-4">{{ total }}</div> 
        <div class="text-muted small">Total</div> 
        </div> 
        <div class="col-4"> 
        <div class="fw-bold fs-4 text-success">{{ completed }}</div> 
        <div class="text-muted small">Concluídas</div> 
        </div> 
        <div class="col-4"> 
        <div class="fw-bold fs-4 text-warning">{{ pending }}</div> 
        <div class="text-muted small">Pendentes</div> 
        </div> 
    </div> 
    </div> 
</div>
</template>