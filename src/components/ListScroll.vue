
<template>
  <div class="viewport" ref="viewport" @scroll="onScroll">
    <!-- 滚动条 -->
    <div class="scrollbar" :style="{height: listHeight + 'px'}"></div>
    <!-- 展示的列表 -->
    <div class="list" ref="list" :style="{ transform: `translateY(${transformOffset}px)` }">
      <div class="row" :style="{height: rowHeight + 'px'}" v-for="(item, index) in showList" :key="index">
        <slot :record="item"></slot>
      </div>
    </div>
    <!-- 加载 -->
    <div class="loading_wrap" v-show="loading">
      <div class="loading">
        <div class="container"></div>
      </div>
      <div>正在加载中</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    // 数据源
    list: {
      type: Array,
      default: () => []
    },
    // 每行的高度
    rowHeight: {
      type: Number,
      default: 200
    },
    // 显示数量
    viewCount: {
      type: Number,
      default: 10
    },
    // 控制loading
    loading: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      viewHeight: 0, // 可视区域的高度
      startIndex: 0, // 开始索引
      endIndex: 0, // 结束索引
      transformOffset: 0, // 列表的偏移量
      isLoading: false // 控制loading
    }
  },
  mounted () {
    this.initData()
  },
  computed: {
    // 展示的数据
    showList () {
      return this.list.slice(this.startIndex, this.endIndex)
    },
    // 列表的总高度
    listHeight () {
      return this.list.length * this.rowHeight
    }
  },
  methods: {
    // 初始化一些数据
    initData () {
      this.endIndex = this.viewCount
      this.viewHeight = this.$refs.viewport.offsetHeight
    },

    // 列表滚动
    onScroll () {
      const scrollTop = this.$refs.viewport.scrollTop // 获取试图往上滚动的高度
      const currentIndex = Math.floor(scrollTop / this.rowHeight) // 计算当前的索引
      // 只在需要变化的时 才重新赋值
      if (this.startIndex !== currentIndex) {
        this.startIndex = currentIndex
        this.endIndex = this.startIndex + this.viewCount// 结束索引
        this.transformOffset = scrollTop - (scrollTop % this.rowHeight)
      }
      // 触底了
      if ((this.viewHeight + scrollTop) === this.listHeight) {
        // 发送触底加载事件
        this.$emit('bottomLoad')
      }
    }
  }
}
</script>

<style scoped>
/*
------最外层容器---------*/
.viewport {
  width: 100%;
  height: 100%;
  background-color: #fff;
  position: relative;
  overflow-y: auto;
}
/*
------列表展示层容器---------*/
.list {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
}
/*
------每行容器---------*/
.row {
  overflow: hidden;
}
/*
------loading样式---------*/
.loading_wrap {
  display: flex;
  justify-content: center;
  align-items: center;
  color: #999;
  padding: 20px 0;

}
.loading {
    box-sizing: border-box;
    width: 20px;
    height: 20px;
    border: 2px solid #ddd;
    border-radius: 50%;
    animation: rotate 1s linear infinite;
    margin-right: 10px;
  }
  .container {
    position: relative;
    top: 50%;
    left: 50%;
    width: 10px;
    height: 10px;
    background-color: #fff;
  }
/*
------loading动画---------*/
@keyframes rotate {
  from {
    transform-origin: center center;
    transform: rotate(0deg);
  }
  to {
    transform-origin: center center;
    transform: rotate(360deg);
  }
}
</style>
