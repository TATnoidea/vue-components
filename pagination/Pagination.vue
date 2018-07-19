<template>
  <ul class='page-list'>
    <!-- 上一页 -->
    <li v-show='currentPage > 1' class='page-item'  @click='changePage(currentPage - 1)'>&lt;</li>
    <!-- 首页 -->
    <li :class="['page-item', currentPage === 1 ? 'active': '']" @click='changePage(1)'>1</li>
    <!-- 省略号 -->
    <li v-show='startPage > 2' class='ellipsis'>...</li>
    <!-- 中间按钮 -->
    <li
    v-show='startPage + index <= endPage && startPage + index > 1'
    :class="['page-item', startPage + index === currentPage ? 'active': '']"
    v-for='(item, index) in paginationSize'
    :key='index'
    @click='changePage(startPage + index)'
    >
    {{ startPage + index }}
    </li>
    <!-- 省略号 -->
    <li v-show='endPage < totalPage - 1' class='ellipsis'>...</li>
    <!-- 末页 -->
    <li v-show='totalPage > 1' :class="['page-item', currentPage === totalPage ? 'active': '']"  @click='changePage(totalPage)'>{{ totalPage }}</li>
    <!-- 下一页 -->
    <li class='page-item' v-show='currentPage < totalPage'  @click='changePage(currentPage + 1)'>&gt;</li>
    <!-- 页面跳转 -->
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
        return this.paginationCount - 2; //除了首页按钮和末尾页按钮，中间的按钮个数
      },
      startPage() {
        let startPage = this.currentPage - Math.floor(this.paginationSize / 2); // 开始页页数 最小值为2
        if(startPage < 2 || this.totalPage - this.paginationSize < 0) { // 当总页面数小于总按钮数时或者开始页小于2，开始页设置为2，因为此时首页按钮是一直显示的
          startPage = 2;
        } else if(startPage + this.paginationSize > this.totalPage) { // 当开始页加上按钮个数超过总页数时，需要修改开始页，为总页数减去总共按钮个数
          startPage = this.totalPage - this.paginationSize;
        }
        return startPage;
      },
      endPage() {
        let endPage = this.startPage + this.paginationSize - 1; // 按钮应该显示末尾页页数 最大值为总页数-1
        if(endPage >= this.totalPage || endPage < this.paginationSize) {  // 当末尾页大于总页数或者末尾页小于中间按钮数按钮数，将末尾页设为最大值。
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
