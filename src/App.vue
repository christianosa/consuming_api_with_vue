<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">

      <img src="./assets/pokeapi_256.png">
      <br>
      <hr> 
        <input type="text" class="input is-rounded" placeholder="Search pokemn by name .." v-model="busca">   
        <button id="btnFilter" class="button is-fullwidth is-success" @click="filter">Search ...</button>
  
      <div v-for="(poke, index) in filtered" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index+1" />
      </div>
    </div>
  </div>
</template>

<script>

  import axios from 'axios';
  import Pokemon from './components/Pokemon';

  export default {
    name: 'App',
    data() {
      return {
        pokemons: [],
        filtered: [],
        busca: ""
      }
    },
    created: function() {
      axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
        this.pokemons = res.data.results;
        this.filtered = res.data.results;
        console.log("peguei a lista de pokemons ..");
      }).catch(err =>{
        console.log(err);
      })
    },
    components: {
      Pokemon
    },
    methods: {
      filter: function() {
        this.filtered = this.pokemons;

        if(this.busca == "" || this.busca == " ") {
           this.filtered = this.pokemons;
        } else {
          
          var newName = this.busca[0].toLowerCase() + this.busca.slice(1);
          this.filtered = this.pokemons.filter(pokemon => pokemon.name == newName)
        }        
      }
    }
    /*
    computed: {
      searchResult: function() {
        
        if(this.busca == "" || this.busca == " ") {
          return this.pokemons;
        } else {
          
          var newName = this.busca[0].toLowerCase() + this.busca.slice(1);
          return this.pokemons.filter(pokemon => pokemon.name == newName)
        }
      }
    } */
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
#btnFilter {
  margin-top: 2%;
  margin-bottom: 2%;
}
</style>
