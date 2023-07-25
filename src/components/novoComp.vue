<template>
    <div v-if="batata">uma prop foi passada, seu valor é: {{ batata }}</div>
    <ul v-for="poke in pokemons">
        <li>{{ poke }}</li>
    </ul>
</template>

<script setup lang="ts">

    let pokemons: Record<string, any> = []

    defineProps<{
        batata?: string
    }>()

    fetch("https://pokeapi.co/api/v2/pokemon/")
        .then(res => res.json())
        .then(json => {
            json.results.map((pokemon:any) => {
                pokemons.push(pokemon.name)
            })
        })

</script>