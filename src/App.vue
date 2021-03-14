<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <h1 class="title is-1">Pokedex Vue.js</h1>
      <input class="input is-rounded" v-model='busca' type="text" placeholder="Qual pokemon você procura?">
      <button style="margin-top: 2%" @click="buscar" class="button is-info is-rounded is-fullwidth">Buscar</button>
      <hr>
      <div v-for="(poke,index) in pokemonsFiltrados" :key="poke.url">
        <Pokemon :numero='index + 1' :nome='poke.name' :url='poke.url' />
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
    return{
      pokemons : [],
      pokemonsFiltrados : [],
      busca: ''
    }
  },
  created: function(){
    axios.get('https://pokeapi.co/api/v2/pokemon?offset=0&limit=151')
      .then(res=>{
        this.pokemons = res.data.results
        this.pokemonsFiltrados = res.data.results
      })
  },
  methods:{
    buscar: function(){
      if(this.busca == '' || !this.busca){
        this.pokemonsFiltrados = this.pokemons
      }else{
        this.pokemonsFiltrados = this.pokemons.filter(pokemon => pokemon.name.includes(this.busca.toLowerCase()));
      }
    }
  },
  components: { Pokemon },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
