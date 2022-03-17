<template>
    <article class="c-pokemon-card">
        <h2 class="c-pokemon-card__title">{{ pokemon.name }}</h2>
        <div class="c-pokemon-card__wrapper">
          <p class="c-pokemon-card__info">Height : {{ pokemonHeight }} <abbr title="feet">ft</abbr></p>
          <p class="c-pokemon-card__info">Weight : {{ pokemonWeight }} <abbr title="pounds">lbs</abbr></p>
        </div>
        <img class="c-pokemon-card__img" :src="pokemonImageUrl + pokemon.id + '.png'" :alt="pokemon.name">
    </article>
</template>

<script>
import axios from 'axios'

export default {
  name: 'PokemonCard',
  props: {
    pokemon: {
      type: Object
    }
  },
  data: () => {
    return {
      pokemonImageUrl: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
      pokemonHeight: '',
      pokemonWeight: ''
    }
  },
  created () {
    this.getExtraInfo()
  },
  methods: {
    /*
    * @desc get the Height and the weight of the Pokemon
    * @return Object
    */
    getExtraInfo () {
      axios
        .get(this.pokemon.url)
        .then(res => {
          this.pokemonHeight = res.data.height
          this.pokemonWeight = res.data.weight
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
}
</script>

<style scoped lang="scss">
    .c-pokemon-card {
        background-color: white;
        border: 1px solid lightgray;
        border-radius: 4px;
        box-shadow: 0px 3px 15px -3px grey;
        box-sizing: border-box;
        color: black;
        display: flex;
        flex-direction: column;
        padding: 0 10px 10px 10px;
        position: relative;

        &__title {
            background-color: #EF534F;
            border-bottom-left-radius: 4px;
            border-bottom-right-radius: 4px;
            color: black;
            font-size: 1.4rem;
            line-height: 1.4rem;
            margin: 0 -14px;
            order: 2;
            padding: 5px;
            text-align: center;
            text-transform: uppercase;
            position: relative;

            &::after {
                content: '';
                background-color: #D32B2A;
                border-radius: 4px;
                bottom: 5px;
                height: 100%;
                left: 0;
                position: absolute;
                width: 100%;
                z-index: -1;
            }
        }

        &__wrapper {
            backdrop-filter: blur(2px);
            background-color: rgba(255, 255, 255, 0.6);
            border-radius: 4px;
            box-sizing: border-box;
            display: flex;
            flex-wrap: wrap;
            flex-direction: column;
            height: 100%;
            justify-content: center;
            left: 0;
            opacity: 0;
            padding: 10px;
            position: absolute;
            top: 0;
            transition: opacity 0.3s ease-in-out;
            width: 100%;
        }

        &__info {
            font-size: 1.4rem;
            font-weight: bold;
            text-align: center;
            margin: 0 0 10px 0;

            &:last-of-type {
                margin-bottom: 0;
            }
        }

        &__img {
            display: block;
            height: auto;
            margin: 0 auto;
            order: 1;
            width: 80%;
        }

        &:hover,
        &:focus {
            .c-pokemon-card__wrapper {
                opacity: 1;
            }
        }
    }
</style>
