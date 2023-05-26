<template>
  <div>
    <h3>vitlist</h3>
    <div class="list" ref="listRef" @scroll.passive="getScroll($event)">
      <div :style="{ height: 40 * Number(options.length )+ 'px', width: '100%' }">
        <div class="item" v-for="(item, index) in options.slice(min - 2, min + 10)" :key="index" :style="{ top: `${40 * Number(item.label)}px` }">
          {{ item.value }}:{{ item.label }}
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { ref } from 'vue'
export default {
  setup() {
    let min = ref(2)
    const getScroll = (event: any) => {
      let scrollTop = event.target.scrollTop
      // 当滚动距离大于2条数据的高度时开始动态计算
      if (scrollTop > 2 * 40) {
        min.value = Math.ceil(scrollTop / 40)
      } else {
        min.value = 2
      }
    }

    return {
      // 构造十万条数据
      options: ref(
        Array.from({ length: 1000 }).map((_, idx) => ({
          value: `Option ${idx + 1}`,
          label: `${idx}`
        }))
      ),
      getScroll,
      min
    }
  }
}
</script>

<style scoped>
 .list {
  margin: 10px auto;
  width: 300px;
  height: 200px;
  overflow: auto;
  position: relative;
  background-color: pink;

}
   .item {
    height: 40px;
    line-height: 40px;
    position: absolute;
    width: 100%;
  }
</style>
