<template>
  <div class="page">
    <!-- <h1 v-show="y > 360">{{ count }}* 2 = {{ double }}</h1>
    <p>{{ x }}:{{ y }}</p>
    <button @click="add">add</button> -->
    <MyList :style="{'height': '100vh'}" />
    <!-- <h3>长列表渲染</h3>
    <ListScroll class="list_scroll" :list="listData" :loading="isLoading" @bottomLoad="onBottomLoad">
      <template v-slot="{ record }">
        <div class="row_content" @click="handleClick(record)">
          <div>{{ record }}</div>
          <img
            class="image"
            src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fsafe-img.xhscdn.com%2Fbw1%2F2076f7ae-d134-4dc4-a865-af1b2029d400%3FimageView2%2F2%2Fw%2F1080%2Fformat%2Fjpg&refer=http%3A%2F%2Fsafe-img.xhscdn.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1680249943&t=7646a71b62c810256a2b414e96106808"
          />
        </div>
      </template>
    </ListScroll> -->
  </div>
</template>

<script>
import { ref, watchEffect, onMounted, onUnmounted, computed } from 'vue';
import MyList  from './components/MyList.vue';
import ListScroll from './components/ListScroll.vue';

function useMouse() {
  let x = ref(0),
    y = ref(0);
  function update(e) {
    x.value = e.pageX;
    y.value = e.pageY;
  }

  onMounted(() => {
    window.addEventListener('mousemove', update);
  });
  onUnmounted(() => {
    window.removeEventListener('mousemove', update);
  });

  return { x, y };
}
export default {
  // setup() {
  //   let count = ref(0);
  //   let add = () => {
  //     count.value++;
  //   };
  //   let double = computed(() => count.value * 2);
  //   let { x, y } = useMouse();
  //   watchEffect(() => {
  //     console.log('count.value++', count.value);
  //   });
  //   return { count, add, double, x, y };
  // },
  data() {
    return {
      listData: [], // 总数据
      isLoading: false // 展示loading
    }
  },
  mounted () {
    this.getListData()
  },
  components: {
    ListScroll,
    MyList
  },
  methods: {
    // 获取数据
    getListData () {
      const count = 20 + this.listData.length
      const start = this.listData.length
      this.isLoading = true
      setTimeout(() => {
        for (let i = start; i < count; i++) {
          this.listData.push(i)
        }
        this.isLoading = false
      }, 1000)
    },
    // 监听触底事件
    onBottomLoad () {
      console.log('触底了')
      if (this.listData.length >= 100) {
        console.log('数据加载完了～')
        return
      }
      // 加载数据
      this.getListData()
    },
    // 监听点击每行
    handleClick (record) {
      console.log(record, 'record')
    }
  }
}
// composition Api
</script>

<style scoped>
.page {
  display: flex;
  flex-direction: column;
  height: 100vh;
}
.list_scroll {
  flex: 1;
}
.row_content {
  width: 100%;
  height: 100%;
  .image {
    display: block;
    width: 300px;
    height: 160px;
    object-fit: cover;
  }
}
</style>
