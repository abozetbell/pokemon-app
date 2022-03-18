<template>
  <div id="app">
    <HeaderComponent/>
    <PokedexComponent :key="refreshComponent" v-if="isDiplayed" :maxItem="150" :itemPerPage="15" :firstLoadItems="50" nav="loadmore"/>
    <PokedexComponent :key="refreshComponent" v-if="!isDiplayed" :maxItem="150" :itemPerPage="30" nav="pagination"/>
    <div class="l-wrapper">
        <ButtonComponent @click.native="isDiplayed = !isDiplayed; refreshComponent++" btnStyle="light" text="navigate by load more" :disabled="isDiplayed"/>
        <ButtonComponent @click.native="isDiplayed = !isDiplayed; refreshComponent++" btnStyle="light" text="navigate by page" :disabled="!isDiplayed"/>
    </div>
  </div>
</template>

<script>
import ButtonComponent from '@/components/tools/ButtonComponent.vue'
import HeaderComponent from '@/components/structure/HeaderComponent.vue'
import PokedexComponent from '@/components/PokedexComponent/PokedexComponent.vue'

export default {
  name: 'App',
  components: {
    ButtonComponent,
    HeaderComponent,
    PokedexComponent
  },
  data: () => {
    return {
      isDiplayed: true,
      refreshComponent: null
    }
  }
}
</script>

<style lang="scss">
:root {
  font-size: 62.25%;
}
body {
  margin:80px 0 0 0;
}
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: left;
    color: black;
    display:flex;
    align-items: center;
    flex-direction: column;
}
.l-wrapper {
    background-color: white;
    box-shadow: 0px 3px 15px -3px grey;
    bottom: 0;
    display: inline-flex;
    flex-basis: 100%;
    justify-content: center;
    position: fixed;
    right: 0;
    padding: 10px 10px 12px;
    width: 100%;

    > * {
        margin: 0 10px;
    }
}
</style>
