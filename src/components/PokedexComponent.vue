<script lang="ts">
import { ref, onMounted, computed } from "vue";

export default {
    name: 'PokemonsList',
    props: {
        abacate: String
    },
    setup() {
        const limite = ref(40)
        const inicio = 0
        const url = ref("https://pokeapi.co/api/v2/pokemon")
        const data = ref<any[]>([])
        const error = ref<any>();

        async function fetchData() {
            return await fetch(`${url.value}?limit=${limite.value}&offset=${inicio}`)
                .then(res => res.json())
                .then(async (json) => {
                    await json.results.map(async (pokemon: any) => {
                        data.value.push(
                            await fetch(pokemon.url)
                                .then(res => res.json())
                        )
                    })

                    return json
                })
                .then((json) => {
                    console.log(data.value)
                    url.value = json.next
                    console.log(url.value)
                }).catch((e) => {
                    error.value = e
                    console.error(new Error(`Houve um erro ao fazer a requisição ${e}`))
                });

        }

        const intersection = new IntersectionObserver((entries) => {
            if (entries[0].isIntersecting) {
                fetchData();
            }
        })

        const sortedPokemons = computed(() => {
            return [...data.value].sort((a, b) => a.id > b.id ? 1 : -1)
        })

        onMounted(() => {
            document.querySelectorAll(".observer").forEach(entrie => {
                intersection.observe(entrie)
            })
        });

        return {
            sortedPokemons,
            error,
            limite,
        };
    }
}
</script>

<template>
    <ul class="lista" v-if="sortedPokemons.length">
        <li :class="pokemon.name + ' pokemon'" :id="pokemon.id" :key="pokemon.id" v-for="pokemon of sortedPokemons">
            <img :src="pokemon.sprites.front_shiny" alt="">
            <p class="nome"><strong>{{ pokemon.name }}</strong></p>
            <p class="id">PokeDex ID: {{ pokemon.id }}</p>
            <p></p>
        </li>
    </ul>

    <p v-if="error">
        Deu problema na requisição, se vira
    </p>

    <div class="observer">
       Carregando...
    </div>
</template>

<style>
.nome {
    text-align: center;
    color: rgb(255, 255, 255);
}

.lista {
    padding-inline-start: 0;
    width: fit-content;
    display: grid;
    grid: auto-flow 200px / repeat(3, 150px);
    margin: 20px auto;
    gap: 20px;
    list-style: none;
}

.id{
    color: white;
}

.pokemon {
    text-align: center;
    background-color: rgb(53, 53, 95);
}

.observer {
    width: 100%;
    margin-top: 30px;
    height: 50px;
    font-weight: bold;
    font-size: 25px;
    text-align: center;
    background-color: rgb(255, 255, 255);
}
</style>