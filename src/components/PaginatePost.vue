<script setup>

defineProps(['inicio','fin', 'maxLength'])
const emit = defineEmits(['nextNombre', 'prevNombre'])

</script>

<template>
    <div class="btn-group" role="group" aria-label="Basic outlined example">
        <button 
            @click="emit('prevNombre')" 
            type="button" 
            class="btn btn-outline-primary"
            :disabled="inicio <= 0"
        >Previus {{ inicio }}
        </button>
        <button 
            @click="emit('nextNombre')" 
            type="button" 
            class="btn btn-outline-primary"
            :disabled="fin >= maxLength"
        >Next {{ fin }}
        </button>
    </div>
</template>