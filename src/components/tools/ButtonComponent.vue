<template>
    <button class="c-button" :class="`c-button--${btnStyle}`" :type="btnType" :value="value || text">
        <slot/> {{text}}
    </button>
</template>

<script>

export default {
  name: 'ButtonComponent',
  props: {
    btnStyle: {
      type: String,
      default: 'dark',
      validator: (value) => [
        'dark',
        'light'
      ].includes(value.toLowerCase())
    },
    btnType: {
      type: String,
      default: 'button',
      validator: (value) => [
        'button',
        'submit',
        'reset'
      ].includes(value.toLowerCase())
    },
    text: {
      type: String,
      default: null
    },
    value: {
      type: String,
      default: null
    }
  }
}
</script>

<style scoped lang="scss">
    .c-button {
        border-radius: 4px;
        border:0;
        padding: 10px;
        text-transform: uppercase;

        &:hover {
            cursor: pointer;
        }

        &__icon {
            display: block;
            height: 24px;
            width: 24px;
        }

        &--dark {
            background-color: #3A5FAA;
            -webkit-box-shadow: 0px 3px 0px 1px #1D2C5D;
            box-shadow: 0px 3px 0px 1px #1D2C5D;
            color: white;
        }

        &--light {
            background-color: whitesmoke;
            -webkit-box-shadow: 0px 3px 0px 1px lightgrey;
            box-shadow: 0px 3px 0px 1px lightgrey;
            color: black;
        }

        &--dark {
          &:disabled,
          &[disabled] {
              background-color: darkgrey;
              -webkit-box-shadow: 0px 3px 0px 1px grey;
              box-shadow: 0px 3px 0px 1px grey;
              color: white;
              cursor: auto;
            }
        }

        &--light {
          &:disabled,
          &[disabled] {
              color: darkgrey;
              cursor: auto;
            }
        }

        &--light {
          &:disabled,
          &[disabled] {

          }
        }
    }
</style>
