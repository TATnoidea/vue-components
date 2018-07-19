<template>
<transition name='fade'>
  <div v-show='visible' class='yx-dialog'>
    <div class='yx-header'>
      {{ title }}
      <button @click='handleClose' class='close'></button>
    </div>
    <div class='yx-main'>
      <slot></slot>
    </div>
    <div class='yx-footer'>
      <slot name='footer'></slot>
    </div>
  </div>
</transition>
</template>

<script>
  export default {
    name: 'YxDialog',
    props: {
      visible: {
        type: Boolean,
        default: false,
      },
      title: {
        type: String,
        required: true,
      },
    },
    methods: {
      handleClose() {
        this.$emit('update:visible', false);
      }
    }
  }
</script>

<style lang="scss" scoped>
.yx-dialog {
  width: 434px;
  border: 1px solid #aaa;
  background-color: #fff;
  .yx-header {
    width: 434px;
    height: 40px;
    box-sizing: border-box;
    padding-left: 20px;
    line-height: 40px;
    border-bottom: 1px solid #ddd;
    font-size: 18px;
    .close {
      width: 40px;
      height: 40px;
      position: absolute;
      top: 0;
      right: 10px;
      @extend .close-icon;
    }
  }
  .yx-main {
    font-size: 16px;
    padding: 20px 0;
  }
  .yx-footer {
    padding: 0 60px 12px 0;
    display: flex;
    justify-content: flex-end;
  }
}
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.5s;
  }
  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }
</style>

