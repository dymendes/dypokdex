<template>
  <div id="pokemon" v-on:click="changeSprite($event)">
    <img class="avatar" :src="pokemon.sprites.front" alt="Imagem do pokemon.">
  
    <div class="informations">
      <small class="index">Nº {{ pokemon.index }}</small>
      <h3 class="name">{{ name }}</h3>
      <small class="type">{{ pokemon.type }}</small>
  </div>
  </div>
</template>

<script>
import axios from "axios"

export default (await import("vue")).defineComponent({
  name: "Pokemon",
  data() {
    return {
      pokemon: {
        index: Number,
        type: String,
        sprites: {
          back: String,
          front: String
        }
      }
    }
  },
  created() {
    axios.get(`${this.link}`).then(res => {
      this.pokemon.index = res.data.id
      this.pokemon.type = res.data.types[0].type.name
      this.pokemon.sprites.back = res.data.sprites.back_default
      this.pokemon.sprites.front = res.data.sprites.front_default
    })
  },
  props: {
    name: String,
    link: String,
  },
  methods: {
    changeSprite($event) {
      if($event.target.src === this.pokemon.sprites.front) {
        $event.target.src = this.pokemon.sprites.back
      } else {
        $event.target.src = this.pokemon.sprites.front
      }
    }
  }
})
</script>

<style scoped>
  #pokemon {
    display: flex;
    flex-direction: column;
    padding: 10px;
    background-color: rgb(160 160 160 / 30%);
    border-radius: 5px;
    cursor: pointer;
    transition: 0.3s ease-in;
  }

  #pokemon:hover {
    transform: translate3d(0, -5px, 0);
    background-color: rgb(160 160 160 / 50%);
  }

  #pokemon .informations {
    height: 100%;
    display: grid;
    gap: 5px;
    margin-top: 10px;
  }

  #pokemon .avatar {
    width: 96px;
    object-fit: scale-down;
  }

  #pokemon .informations .index {
    color: #ef4036;
  }

  #pokemon .informations .name {
    color: rgb(78 78 78);
  }

  #pokemon .informations .type {
    width: fit-content;
    display: flex;
    background-color: rgb(128 128 128 / 30%);
    border-radius: 5px;
    padding: 5px 15px;
    text-align: center;
    align-items: center;  
  }
</style>