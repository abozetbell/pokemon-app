<template>
    <div class="c-list-tool">
        <!-- List tools -->
        <div class="c-list-tool__settings">
            <ul class="c-list-tool__settings__list">
                <li class="c-list-tool__settings__item">
                    <ButtonComponent @click.native="layout = 'grid'" btnStyle="light" icon="grid">
                        <img src="../../assets/grid-icon.svg" class="c-button__icon" alt="grid icon" aria-hidden="true" />
                    </ButtonComponent>
                </li>
                <li class="c-list-tool__settings__item">
                    <ButtonComponent @click.native="layout = 'grid-small'" btnStyle="light" icon="grid-small">
                        <img src="../../assets/grid-small-icon.svg" class="c-button__icon" alt="small grid icon" aria-hidden="true" />
                    </ButtonComponent>
                </li>
            </ul>
        </div>
        <!-- List of Pokemons -->
        <ul class="c-list-tool__list" :class="`c-list-tool__list--${layout}`">
            <li class="c-list-tool__item" v-for="(item, index) in list" :key="index">
                <PokemonCard :pokemon="item"/>
            </li>
        </ul>
    </div>
</template>

<script>
import ButtonComponent from '@/components/tools/ButtonComponent.vue'
import PokemonCard from '@/components/PokedexComponent/PokemonCard.vue'

export default {
  name: 'ListTool',
  components: {
    ButtonComponent,
    PokemonCard
  },
  props: {
    list: {
      type: Array
    }
  },
  data: () => {
    return {
      layout: 'grid'
    }
  }
}
</script>

<style scoped lang="scss">
    .c-list-tool {
        &__list {
            list-style: none;
            padding: 0;
            display: flex;
            margin-left:-10px;
            margin-right:-10px;

            &--grid,
            &--grid-small {
                flex-wrap: wrap;
            }

        }

        &__settings {
            &__list {
                display: flex;
                justify-content: flex-end;
                list-style: none;
                margin: 0 0 20px;
            }
            &__item:not(:last-of-type) {
                margin-right: 20px;
            }
        }

        &__item {
            box-sizing: border-box;
            margin-bottom: 20px;

            .c-list-tool__list--grid &,
            .c-list-tool__list--grid-small & {
                align-items: flex-start;
                justify-content: space-between;
                padding: 0 10px;
            }

            .c-list-tool__list--grid & {
                width: calc(100% * 1/5);
            }

            .c-list-tool__list--grid-small & {
                width: calc(100% * 1/3);
            }

        }
    }
</style>
