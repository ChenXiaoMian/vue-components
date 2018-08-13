<template>
  <div class="page-wrap">
    <ul>
      <li class="page-prev" @click="selectPage(value - 1)"><i class="iconfont">&lt;</i></li>
      <li class="page-item"
          v-for="(page, index) in pages"
          :key="index"
          :class="{'active': page.active}"
          @click="selectPage(page.number)"
      >{{ page.text }}</li>
      <li class="page-next" @click="selectPage(value + 1)"><i class="iconfont">&gt;</i></li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    // 总页数
    pageCount: Number,
    // 当前页码
    value: Number,
    // value: Number,
    // prevText: String,
    // nextText: String,
    // pageCount: Number,
    // forceEllipses: Boolean,
    // mode: {
    //   type: String,
    //   default: 'multi'
    // },
    // 每页显示记录数
    itemsPerPage: {
      type: Number,
      default: 10
    },
    // 显示的页码个数
    showPageSize: {
      type: Number,
      default: 5
    },
    // 总记录数
    totalItems: {
      type: Number,
      default: 0
    }
  },
  computed: {
    computedPageCount () {
      const count = this.pageCount || Math.ceil(this.totalItems / this.itemsPerPage)
      return count
    },
    pages () {
      const pages = []
      const pageCount = this.computedPageCount

      let startPage = 1
      let endPage = pageCount
      // 配置显示页码的数量
      const isMaxSized = this.showPageSize !== undefined && this.showPageSize < pageCount

      if (isMaxSized) {
        // Current page is displayed in the middle of the visible ones
        startPage = Math.max(this.value - Math.floor(this.showPageSize / 2), 1);
        endPage = startPage + this.showPageSize - 1;
        // Adjust if limit is exceeded
        if (endPage > pageCount) {
          endPage = pageCount;
          startPage = endPage - this.showPageSize + 1;
        }
      }

      for(let number = startPage; number <= endPage; number++){
        let page = this.makePage(number, number, number === this.value)

        pages.push(page)
      }
      console.log(pages)

      if (isMaxSized && this.showPageSize > 0 && this.forceEllipses) {
        if (startPage > 1) {
          const previousPageSet = this.makePage(startPage - 1, '...', false);
          pages.unshift(previousPageSet);
        }
        if (endPage < pageCount) {
          const nextPageSet = this.makePage(endPage + 1, '...', false);
          pages.push(nextPageSet);
        }
      }

      return pages
    }
  },
  methods: {
    makePage(number, text, active){
      return {
        number,
        text,
        active
      }
    },
    selectPage (page) {
      page = Math.max(1, page)
      page = Math.min(this.computedPageCount, page)
      if (this.currentPage !== page) {
        this.$emit('input', page)
        this.$emit('change', page)
      }
    }
  }
}
</script>

<style>
.page-wrap {
  width: 5.1rem;
  height: .8rem;
  margin: .2rem auto;
}
.page-wrap li {
    float: left;
    display: block;
    width: .72rem;
    height: .72rem;
    background: #FFFFFF;
    line-height: .72rem;
    text-align: center;
    border: .02rem solid #dcdee2;
    -webkit-box-shadow: .04rem .04rem .06rem #888888;
            box-shadow: .04rem .04rem .06rem #888888;
    border-right: 0;
}
.page-wrap li:active {
      color: #3677E0;
}
.page-wrap .page-prev {
    border-top-left-radius: 50%;
    border-bottom-left-radius: 50%;
}
.page-wrap .page-next {
    border-right: .02rem solid #dcdee2;
    border-top-right-radius: 50%;
    border-bottom-right-radius: 50%;
}
.page-wrap .page-item.active {
    background: #3677E0;
    border-color: #3677E0;
    color: #FFFFFF;
}
</style>
