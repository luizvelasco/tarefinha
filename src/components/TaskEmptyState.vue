<script setup>
const props = defineProps(['message'])
</script>

<template>
    <div class="card bg-light"> 
      <div class="card-body text-center py-5"> 
        <p class="text-muted mb-0">
          {{ props.message }}
        </p> 
      </div> 
    </div>
</template>