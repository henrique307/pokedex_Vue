<script setup lang="ts">
    const ok: string = "teste"

    defineProps<{
        batata?: string
    }>()
</script>

<template>
    <div v-if="batata">uma prop foi passada {{ batata }}</div>
    <div>{{ ok }}</div>
</template>