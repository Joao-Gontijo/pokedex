<template>
  <div id="app">
    
    <div class="column">
      <figure >
        <img src="./assets/pokemon-logo.png">
      </figure>
      
      <hr>
      <h4>Pokédex - João Gontijo</h4>
      <input class="input is-rounded mt-4" type="text" placeholder="Buscar Pokémon pelo nome" v-model="busca">
      <button class="button is-fullwidth is-success mt-4" @click="buscar">Buscar</button>
      <div class="columns  is-multiline">
      <div class="column is-one-quarter" v-for="(poke) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="poke.id+1"/>
      </div>
      </div>
    </div>
    
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';
export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      filteredPokemons: [], //lista auxiliar de pokémon para busca
      busca: ''
    }
  },
  created: function(){ //assim que a requisição acabar, vai passar os dados para a função
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    })
  },
  components:{
    Pokemon
  },
  methods:{
    buscar: function(){
      this.filteredPokemons = this.pokemons;
      if(this.busca == '' || this.busca == ' '){
          this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca.toLowerCase());
      }
    }
  },
  computed:{
    // resultadoBusca: function() {
    //   if(this.busca == '' || this.busca == ' '){
    //     return this.pokemons;
    //   } else {
    //     return this.pokemons.filter(pokemon => pokemon.name == this.busca);
    //   }
    // }

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: rgb(184, 73, 73);
}
</style>
