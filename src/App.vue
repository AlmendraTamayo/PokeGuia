<template>
  <div id="app">
    <img src="./assets/pokemon-logo.png" />
    <h1>PokeGuía</h1>
    <div>
      <label>Nombre: </label>
      <input v-model="nombrePokemon" />
      <button @click="buscarPokemon">Buscar</button>
      <!--       <pre>{{ pokemonSpriteFrontDefault }}</pre> -->
    </div>
    <div>
      <img :src="frontDefault" />
    </div>
    <div>
      <h2>Movimientos</h2>
      <ul v-for="(movimiento, $index) in moves" :key="$index">
        <li>{{ movimiento.move.name }}</li>
      </ul>
    </div>
    <div>
      <h2>Habilidades</h2>
      <ul v-for="(habilidad, $index) in abilities" :key="$index">
        <li>{{ habilidad.ability.name }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    nombrePokemon: '',
    pokemon: {},
  }),
  methods: {
    buscarPokemon() {
      if (this.nombrePokemon !== '') {
        this.getPokemon(this.nombrePokemon)
      }
    },
    async getPokemon(nombrePokemon) {
      try {
        let response = await fetch(`https://pokeapi.co/api/v2/pokemon/${nombrePokemon}`)
        let data = await response.json()
        this.pokemon = data
      } catch (e) {
        console.error(e)
      }
    },
  },
  computed: {
    frontDefault() {
      return this.pokemon?.sprites?.front_default ?? ''
    },
    moves() {
      return this.pokemon?.moves ?? []
    },
    abilities() {
      return this.pokemon?.abilities ?? []
    },
    //pokemonSpriteFrontDefault() {
    //return this.pokemon.sprites.other.dream_world.front_default
    /* return (
        (this.pokemon &&
          this.pokemon.sprites &&
          this.pokemon.sprites.other &&
          this.pokemon.sprites.other.dream_world &&
          this.pokemon.sprites.other.dream_world.front_default) ||
        'sin info'
      ) */
    /*  if (
        'sprites' in this.pokemon &&
        'other' in this.pokemon.sprites &&
        'dream_world' in this.pokemon.sprites.other &&
        'front_default' in this.pokemon.sprites.other.dream_world
      ) {
        return this.pokemon.sprites.other.dream_world.front_default
      } else {
        return 'sin info'
      } */
    /*   return this.pokemon?.sprites?.other?.dream_world?.front_default ?? 'sin info'
    }, */
  },
  created() {
    this.nombrePokemon = 'pikachu'
    this.buscarPokemon()
  },
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
ul {
  list-style: none;
}
</style>
