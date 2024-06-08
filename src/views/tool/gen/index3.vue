<template>
<div class="app-container">
  这是测试页面
</div>

</template>

<script setup name="operationDetails">
import { nextTick, ref, onMounted, toRaw,reactive } from 'vue'

watchEffect(() => {
    console.log('onMounted nextTick ')
})



onMounted(() => {
    // Vue.js 提供的一个延迟执行机制，可以在 DOM 更新完成后执行回调函数
    nextTick(() => {
      console.log('onMounted nextTick ')
    })
})



</script>

<style scoped>

</style>
