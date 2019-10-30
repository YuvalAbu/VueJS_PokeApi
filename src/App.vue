<template>
  <v-app>
    <v-content>
      <PokemonList :imageUrl="imageUrl" :apiUrl="apiUrl"/>
    </v-content>
  </v-app>
</template>

<script>
import PokemonList from './components/PokemonList';

export default {
  name: 'App',
  components: {
    PokemonList,
  },
  data: () => ({
    imageUrl: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/",
    apiUrl: "https://pokeapi.co/api/v2/pokemon/"
  }),
};
</script>