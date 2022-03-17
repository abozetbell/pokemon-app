<template>
  <div class="c-pokedex">
    <!-- List -->
    <ListComponent :list="_pokemonsToDisplay"/>
    <!-- Load more -->
    <ButtonComponent v-if="_showLoadMore" @click.native="itemsDisplayed += itemPerPage" text="Load more"/>
    <!-- Pagination -->
    <nav class="c-pagination" v-if="_showPagination">
        <ul class="c-pagination__list">
            <li class="c-pagination__item">
                <ButtonComponent :disabled="page <= 1" class="c-pagination__link c-pagination__link--text" @click.native="page--" text="Previous"/>
            </li>
            <li class="c-pagination__item">
                <ButtonComponent :disabled="page === pageNumber" :text="`${pageNumber}`" v-for="pageNumber in pages" :value="`page ${pageNumber}`" :key="pageNumber" class="c-pagination__link" @click.native="page = pageNumber"/>
            </li>
            <li class="c-pagination__item">
                <ButtonComponent :disabled="page > pages.length - 1" class="c-pagination__link c-pagination__link--text" @click.native="page++" text="Next"/>
            </li>
        </ul>
    </nav>
  </div>
</template>

<script>
import axios from 'axios'
import ButtonComponent from '@/components/tools/ButtonComponent.vue'
import ListComponent from '@/components/tools/ListComponent.vue'

export default {
  name: 'PokemonList',
  components: {
    ButtonComponent,
    ListComponent
  },
  props: {
    firstLoadItems: {
      type: Number,
      default: 20
    },
    itemPerPage: {
      type: Number,
      default: 5
    },
    maxItem: {
      type: Number,
      default: 150
    },
    nav: {
      type: String,
      require: true,
      validator: (value) => [
        'loadmore',
        'pagination'
      ].includes(value.toLowerCase())
    }
  },
  data: () => {
    return {
      pokemonApiURL: 'https://pokeapi.co/api/v2/pokemon/',
      pokemons: [],
      itemsDisplayed: '',
      nextUrl: '',
      offset: 0,
      currentUrl: '',
      page: 1,
      pages: []
    }
  },
  methods: {
    /*
    * @desc get all the Pokemon at once
    * @return Object
    */
    getData () {
      axios
        .get(this.pokemonApiURL, {
          params: {
            offset: this.offset,
            limit: this.maxItem
          }
        })
        .then(res => {
          res.data.results.forEach(pokemon => {
            pokemon.id = pokemon.url.split('/').filter(urlPart => urlPart !== '').pop()
            this.pokemons.push(pokemon)
          })
        })
        .catch(err => {
          console.log(err)
        })
    },
    /*
    * @desc set the number of page needed based on
    * the maximum item and item per page desired
    */
    setNumberOfPages () {
      const NUMBER_OF_PAGES = Math.ceil(this.maxItem / this.itemPerPage)
      for (let index = 1; index <= NUMBER_OF_PAGES; index++) {
        this.pages.push(index)
      }
    },
    /*
    * @desc update the list of pokemon to load the right pokemons for the right page
    * @param 'pokemons' - Object containing the list of pokemons
    * @return Object
    */
    paginate (pokemons) {
      const PAGE = this.page
      const PER_PAGE = this.itemPerPage
      const START_FROM = (PAGE * PER_PAGE) - PER_PAGE
      const END_TO = (PAGE * PER_PAGE)
      return pokemons.slice(START_FROM, END_TO)
    }
  },
  created () {
    this.itemsDisplayed = this.firstLoadItems
    this.getData()
    this.setNumberOfPages()
    this.currentUrl = this.pokemonApiURL
  },
  computed: {
    /*
    * @desc set the right list to use in the listComponent depending of the navigation type
    * @return Object
    */
    _pokemonsToDisplay () {
      return this.nav === 'loadmore' ? this.pokemons.slice(this.offset, this.itemsDisplayed) : this.paginate(this.pokemons)
    },
    /*
    * @desc allow to show the 'load more' button or not
    * @return Boolean
    */
    _showLoadMore () {
      return this.itemsDisplayed < this.pokemons.length && this.nav === 'loadmore'
    },
    /*
    * @desc allow to show the pagination or not
    * @return Boolean
    */
    _showPagination () {
      return this.nav === 'pagination'
    }
  }
}
</script>

<style scoped lang="scss">
    .c-pokedex {
        display: flex;
        flex-direction: column;
        flex-basis: 70%;
        padding-bottom: 90px;
        width: 70%;
    }
    // Pagination
    .c-pagination {
        width: 100%;
    }
    .c-pagination__list {
        list-style: none;
        padding: 0;
        margin: 0;
        text-align: center;
    }
    .c-pagination__item {
        display: inline-block;
        margin: 0 5px 5px 5px;
    }
    .c-pagination__link {
        display: inline-block;
        margin: 0 5px;

        &--text {
            min-width: 100px;
        }
    }
</style>
