<template>
  <div id="App">
    <div class="search">
      <h1>Pokeguide</h1>
      <label for="pokemon">Name or id:</label>
      <input
        type="text"
        name="pokemon"
        v-model="nombrePokemon"
        @keyup.enter="peticionApi"
        autofocus
      />
      <h5 v-if="nombrePokemon != '' ">Press 'enter' key to search</h5>
    </div>
    <hr />
    <section class="info">
      <h2>{{pokeName}}</h2>
      <img :src="pokeImg" alt />

      <div class="movesAndAbilities">
        <div class="moves">
          <h3>Moves</h3>
          <ul>
            <li v-for="(move , i) in pokeMoves" :key="i">{{move.move.name}}</li>
          </ul>
        </div>

        <div class="abilities">
          <h3>Abilities</h3>
          <ul>
            <li v-for="(ability , i) in pokeAbilities" :key="i">{{ability.ability.name}}</li>
          </ul>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "pokeguía",
  data() {
    return {
      nombrePokemon: "",
      pokemon: {
        name: "",
        sprites: {
          front_default: "",
        },
        moves: [],
        abilities: [],
      },
    };
  },
  methods: {
    peticionApi() {
      fetch(this.url)
        .then((res) => res.json())
        .then((data) => {
          console.log(data);
          this.pokemon = data;
          this.nombrePokemon = "";
        })
        .catch((error) => console.log(`Hubo un error: \n${error}`));
    },
  },
  created() {
    this.peticionApi();
  },
  computed: {
    url() {
      return this.nombrePokemon.trim() == ""
        ? `https://pokeapi.co/api/v2/pokemon/pikachu`
        : `https://pokeapi.co/api/v2/pokemon/${this.nombrePokemon
            .toLowerCase()
            .trim()}`;
    },
    pokeName() {
      return this.pokemon.name.toUpperCase();
    },
    pokeImg() {
      return this.pokemon.sprites.front_default;
    },
    pokeMoves() {
      return this.pokemon.moves;
    },
    pokeAbilities() {
      return this.pokemon.abilities;
    },
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  box-sizing: border-box;
}
#app {
  font-family: Helvetica, sans-serif;
  text-align: center;
  min-height: 100vh;
  background-color: #eee;
}
.search {
  background-color: #ff1f1f;
  padding: 30px 0;
  h1 {
    padding: 10px;
  }
  h5 {
    color: #555;
  }
}
.info {
  padding: 30px 0;

  img {
    width: 25%;
  }
  .movesAndAbilities {
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
    h3 {
      text-align: left;
      padding: 16px 0;
    }
    .moves {
      ul {
        columns: 4;
      }
    }
  }
}
ul {
  li {
    text-align: left;
    padding: 3px 0;
  }
}
</style>
