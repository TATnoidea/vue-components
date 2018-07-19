<template>
  <div v-show='visible' class='dialog'>
    <header>
      <slot name='header'></slot>
    </header>
    <main>
      <slot name='main'></slot>
    </main>
    <footer>
      <slot name='footer'></slot>
    </footer>
  </div>
</template>

<script>
  export default {
    name: 'YxDialog',
    props: {
      visible: {
        type: Boolean,
        default: false,
      }
    },
    data() {
      return {
      }
    },
  }
</script>

<style lang="scss" scoped>
.dialog {
  border: 1px solid #aaa;
  background-color: #fff;
  header {
    width: 434px;
    height: 40px;
    padding-left: 20px;
    line-height: 40px;
    border-bottom: 1px solid #ddd;
    font-size: 18px;
  }
  main {
    font-size: 16px;
  }
  footer {
    padding: 0 60px 12px 0;
    display: flex;
    justify-content: flex-end;
  }
}
</style>

