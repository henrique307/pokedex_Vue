<script setup lang="ts">
  import PokemonList from './components/PokedexComponent.vue'
  import SearchBar from './components/SearchBar.vue'
</script>

<template>
  <header>

    <div class="wrapper">
      <SearchBar />

      <PokemonList />
      
      <!-- <nav>
        <HelloWorld msg="Olá mundo!" />
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
        <RouterView />
      </nav> -->
    </div>
  </header>

</template>

<style scoped>
*{
  padding: 0;
  margin:0
}
  .wraper{
    width: 100%;
  }
</style>
