<template>
  <h1 v-if="!pokemon">Espere por favor...</h1>
  
  <div v-else>
    <h1>¿Quién es este Pokémon?</h1>
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
    <PokemonOptions :pokemons="pokemonArr" @selection="checkAnswer"/>

    <div v-if="showAnswer">
      <h2 class="fade-in">{{ message }}</h2>
      <button @click="newGame" class="btn btn-answer">
        Nuevo juego
      </button>
    </div>

  </div>
</template>

<script>

import PokemonPicture from "@/components/PokemonPicture.vue"
import PokemonOptions from "@/components/PokemonOptions.vue"
import getPokemonOptions from "../helpers/getPokemonOptions"


export default {
    components:{
        PokemonPicture,
        PokemonOptions
    },
    data(){
      return {
        pokemonArr: [],
        pokemon: null,
        showPokemon:false,
        showAnswer:false,
        message:""
      }
    },
    methods:{
      async getPokemons(){
        this.pokemonArr = await getPokemonOptions()
        const randInt = Math.floor(Math.random() * 4)
        this.pokemon = this.pokemonArr[randInt]
      },
      checkAnswer(pokemonId){
        this.showPokemon = true
        this.showAnswer = true

        if(this.pokemon.id === pokemonId){
          this.message = `Excelente! 🎉😄 Era ${this.pokemon.name}`  
        }else{
          this.message = `Oops, lo siento! 🥺 Era ${this.pokemon.name}`
        }
      },
      newGame(){
        this.showPokemon = false
        this.showAnswer = false
        this.pokemonArr = []
        this.pokemon = null
        this.getPokemons()
      }
    },
    mounted(){
      this.getPokemons()
    }
}
</script>

<style scoped>
.btn-answer{
  background-color: #4CAF50; /* Green */
  border: none;
  color: white;
  padding: 16px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  -webkit-transition-duration: 0.4s; /* Safari */
  transition-duration: 0.4s;
  cursor: pointer;
  border-radius: 10px;
}

.btn{
  background-color: white; 
  color: black; 
  border: 2px solid #4CAF50;
}

.btn:hover{
  background-color: #4CAF50;
  color: white;
}
</style>