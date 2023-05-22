<template>
  <div id="app">
    <Header v-on:search="search($event)"/>

    <main class="pokedex">
      <Pokemon v-for="pokemon in pokemonSearch" :key="pokemon.url" :name="pokemon.name" :link="pokemon.url"/>

      <button class="showmore" v-show="limit < 151 && !query.length" v-on:click="showmore">Show more pokemons</button>
    </main>
  </div>
</template>

<script>
import axios from "axios"

import Header from "./components/Header.vue"
import Pokemon from "./components/Pokemon.vue"

export default (await import("vue")).defineComponent({
  name: "App",
  components: {
    Header,
    Pokemon
  },
  data() {
    return {
      limit: 35,
      term: String,
      query: "",
      pokemons: Array,
      pokemonSearch: Array
    }
  },
  created() {
    axios.get(`https://pokeapi.co/api/v2/pokemon?limit=35&offset=0`).then(res => {
      this.pokemons = res.data.results
      this.pokemonSearch = this.pokemons
    })
  },
  methods: {
    search($event) {
      this.query = $event.search

      if(this.query === "" || this.query === " ") {
        return this.pokemonSearch = this.pokemons
      } else {
        this.term = new RegExp(this.query , "i")
        return this.pokemonSearch = this.pokemons.filter(pokemon => this.term.test(pokemon.name))
      }
    },
    showmore() {
      this.limit + 35 > 151 ? this.limit = 151 : this.limit += 35

      axios.get(`https://pokeapi.co/api/v2/pokemon?limit=${this.limit}&offset=0`).then(res => {
        this.pokemons = res.data.results
        this.pokemonSearch = this.pokemons
      })
    }
  }
})
</script>

<style>
  * {
    font-family: 'Varela Round', sans-serif;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    text-decoration: none;
    color: black;
  }

  #app {
    display: grid;
  }

  #app .pokedex {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 25px;
    padding: 25px;
    justify-self: center;
  }

  #app .pokedex .showmore {
    width: 200px;
    justify-self: center;
    padding: 15px;
    grid-column: span 7;
    cursor: pointer;
    outline: none;
    border: 2px solid;
    border-radius: 5px;
    color: #ef4036;
    transition: 0.2s ease-in;
  }

  #app .pokedex .showmore:hover {
    transform: translate3d(0, -5px, 0);
  }
</style>
