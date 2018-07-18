<template>
  <ul class='page-list'>
    <li v-show='currentPage > 1' class='page-item'  @click='changePage(currentPage - 1)'>&lt;</li>
    <li :class="['page-item', currentPage === 1 ? 'active': '']" @click='changePage(1)'>1</li>
    <li v-show='startPage > 2' class='ellipsis'>...</li>
    <li
    v-show='startPage + index <= endPage && startPage + index > 0'
    :class="['page-item', startPage + index === currentPage ? 'active': '']"
    v-for='(item, index) in paginationSize'
    :key='index'
    @click='changePage(startPage + index)'
    >
    {{ startPage + index }}
    </li>
    <li v-show='endPage < totalPage - 1' class='ellipsis'>...</li>
    <li :class="['page-item', currentPage === totalPage ? 'active': '']"  @click='changePage(totalPage)'>{{ totalPage }}</li>
    <li class='page-item' v-show='currentPage < totalPage'  @click='changePage(currentPage + 1)'>&gt;</li>
    <li class='jump-page'>
      <span>跳转至第</span>  
      <input v-model.number='jumpPage' type='text' maxlength="5">
      <span>页</span> 
      <button @click='jump()'>确定</button>
    </li>
  </ul>
</template>

<script>
  export default {
    name: 'yx-paginiation',
    props: {
      totalPage: {
        type: Number,
        required: true,
      },
      currentPage: {
        type: Number,
        default: 1,
      },
      paginationCount: {
        type: Number,
        default: 5
      }
    },
    data() {
      return {
        jumpPage: ''
      }
    },
    computed: {
      paginationSize() {
        return this.paginationCount - 2;
      },
      startPage() {
        let startPage = this.currentPage - Math.floor(this.paginationSize / 2);
        if(startPage < 2 || this.totalPage - this.paginationSize < 0) {
          startPage = 2;
        } else if(startPage + this.paginationSize > this.totalPage) {
          startPage = this.totalPage - this.paginationSize;
        }
        return startPage;
      },
      endPage() {
        let endPage = this.startPage + this.paginationSize - 1;
        if(endPage >= this.totalPage || endPage < this.paginationSize) {
          endPage = this.totalPage - 1;
        }
        return endPage;
      }
    },
    methods: {
      changePage(page) {
        if(page > this.totalPage) {
          page = totalPage;
        } else if(page < 1) {
          page = 1;
        }
        this.$emit('changePage', page);
      },
      jump() {
        if(this.jumpPage > 0 && this.jumpPage <= this.totalPage) {
          this.changePage(this.jumpPage);
          this.jumpPage = '';
        } else if (this.jumpPage > this.totalPage) {
          this.changePage(this.totalPage);
          this.jumpPage = '';
        } else {
          this.changePage(1);
          this.jumpPage = '';
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  .page-list {
    display: flex;
    .page-item {
      width: 30px;
      height: 30px;
      text-align: center;
      line-height: 30px;
      box-sizing: border-box;
      border: 1px solid #ddd;
      margin-right: 10px;
      cursor: pointer;
      &.active {
        background-color: #00a1e9;
        color: #fff;
        border: 0;
      }
    }
    .jump-page {
      height: 30px;
      line-height: 30px;
      color: #333;
      input {
        width: 54px;
        height: 30px;
        margin: 0 10px;
        padding: 0 8px;
        box-sizing: border-box;
        border: 1px solid #ddd;
        &::-webkit-outer-spin-button,
        &::-webkit-inner-spin-butto {
          -webkit-appearance: none;
        }
        &[type='number'] {
          -moz-appearance: textfield;
        }
      }
      button {
        width: 54px;
        height: 30px;
        cursor: pointer;
        box-sizing: border-box;
        border: 1px solid #ddd;
      }
    }
    .ellipsis {
      margin-right: 10px;
    }
  }
</style>
