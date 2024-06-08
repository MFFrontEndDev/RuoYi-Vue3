<template>
<div class="app-container">
  页面C
</div>

</template>

<script setup name="operationDetails">
import { nextTick, ref, onMounted, toRaw,reactive } from 'vue'
const { proxy } = getCurrentInstance();

watchEffect(() => {
    console.log('onMounted nextTick ')
})



onMounted(() => {
    // Vue.js 提供的一个延迟执行机制，可以在 DOM 更新完成后执行回调函数
    nextTick(() => {
      console.log('onMounted nextTick ')
    })
})

/** 测试跳转 changyou  */
function resetQuery() {

  proxy.$router.push({
        path: '/tool/gen-edit/c',
        query: {
            rowId: 1
        }
    });
}

</script>

<style scoped>

</style>
