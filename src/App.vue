<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <input type="text" class="input is-rounded" placeholder="Buscar Pokemon" v-model="busca">
    <button class="button is-fullwidth button is-success" id="buscaBtn">Busca</button>
        <div v-for="(poke, index) in resultadoBusca" :key="poke.url">
          <Pokemon :name="poke.name" :url="poke.url" :num="index+1"/>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/pokemon';
export default {
  name: 'App',
  data(){
    return{
      pokemons:[],
      busca:''
    }
  },
  //EM CREATED FAZ-SE REQUISIÇÕES QUE DEVEM SER CARREGADAS
  //NO INICIO DA PÁGINA
  created: function(){
    axios.get(' https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(res =>{
      this.pokemons = res.data.results;
    })
  },
  components:{
    Pokemon
  },
  computed:{
    resultadoBusca: function(){
      if(this.busca == '' || this.busca === " " ){
        return this.pokemons;
      }else{
        return this.pokemons.filter(pokemon => pokemon.name == this.busca.toLowerCase())
      }
    }
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
  margin-top: 60px;
}
#buscaBtn{
  margin-top: 2%;
}
</style>
