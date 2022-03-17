<template>
  <header class="c-header">
    <img src="../../assets/pokemon-logo.svg" class="c-header__logo" alt="logo Pikachu"/>
  </header>
</template>

<script>

export default {
  name: 'HeaderComponent'
}
</script>

<style scoped lang="scss">
.c-header {
    align-items: center;
    background-color: whitesmoke;
    box-sizing: border-box;
    border-bottom: 1px solid lightgrey;
    box-shadow: 0px 3px 15px -3px grey;
    display: flex;
    height: 60px;
    justify-content: center;
    position: fixed;
    top: 0;
    width: 100%;
    z-index: 9;

    &__logo {
        height: 100%;
    }
}
</style>
