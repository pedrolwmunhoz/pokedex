<template>
  <div id="app">
    <Pokemon />
  </div>
</template>

<script>
import Pokemon from './components/PokemonHome.vue'

export default {
  components: {
    Pokemon
  }
}
</script>

<style lang="scss">

  body {
    margin: 0;
    padding: 0;
  }
</style>