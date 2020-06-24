<!--
 * @Author: RONGWEI PENG
 * @Date: 2020-06-24 21:45:56
 * @LastEditors: Do not edit
 * @LastEditTime: 2020-06-24 22:10:17
 * @FilePath: /vite-demo-project/src/App.vue
-->
<template>
  <div>
    <h1 v-show="y > 360">{{ count }}* 2 = {{ double }}</h1>
    <p>{{ x }}:{{ y }}</p>
    <button @click="add">add</button>
  </div>
</template>

<script>
import { ref, watchEffect, onMounted, onUnmounted, computed } from 'vue';
// composition Api
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
  setup() {
    let count = ref(0);
    let add = () => {
      count.value++;
    };
    let double = computed(() => count.value * 2);
    let { x, y } = useMouse();
    watchEffect(() => {
      console.log('count.value++', count.value);
    });
    return { count, add, double, x, y };
  },
};
</script>

<style scoped></style>
