<template>
  <button :class="[!type? '':'btn-'+type ]">
    <slot></slot>
  </button>
</template>

<script>
  export default {
    name: 'yx-button',
    props: {
      type: {
        type: String,
        default: ''
      }
    },
  }
</script>

<style lang="scss" scoped>
  button {
    width: 80px;
    height: 34px;
    line-height: 34px;
    color: #333;
    font-size: 18px;
    cursor: pointer;
    &.btn-confirm {
      background-color: #00a1e9;
      color: #fff;
    }
    &.btn-cancel {
      border: 1px solid #aaa;
    }
    &.btn-warn {
      border: 1px solid #fe1c1c;
    }
  }
</style>

