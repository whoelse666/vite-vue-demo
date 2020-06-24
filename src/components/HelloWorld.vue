<!--
 * @Author: RONGWEI PENG
 * @Date: 2020-06-24 21:45:56
 * @LastEditors: Do not edit
 * @LastEditTime: 2020-06-24 21:47:15
 * @FilePath: /vite-demo-project/src/components/HelloWorld.vue
--> 
<template>
  <h1>{{ msg }}</h1>
  <button @click="count++">count is: {{ count }}</button>
  <p>Edit <code>components/HelloWorld.vue</code> to test hot module replacement.</p>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  data() {
    return {
      count: 0,
    };
  },
};
</script>
