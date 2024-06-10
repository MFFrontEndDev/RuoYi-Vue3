<template>
<div class="app-container">
  页面C


  <!-- <el-button type="primary" @click="openDialog">打开弹框</el-button>
  <el-dialog
    title="选择项"
    v-model="dialogVisible"
    width="30%"
    @close="handleClose">
    <span>请选择一个选项：</span>
    <el-select v-model="selectedOption" placeholder="请选择">
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value">
      </el-option>
    </el-select>
    <span slot="footer" class="dialog-footer">
      <el-button @click="dialogVisible = false">取消</el-button>
      <el-button type="primary" @click="handleConfirm">确定</el-button>
    </span>
  </el-dialog> -->

  <el-tooltip v-if="branch && (branch + ' ' + commitNum).length > 13"
            placement="top"
            popper-class="tooltip-width" effect="dark">
    <template #content>
      <div v-html="codeInfoDetail.replace(/\n/g, '<br>')"></div>
    </template>
    <span>
      {{ branch }}
      <el-button type="primary" link >{{ formattedBranchAndCommit(branch, commitNum) }}</el-button>
      <!-- <span v-html="formattedBranchAndCommit(branch, commitNum)"></span> -->
    </span>
  </el-tooltip>
  <!-- <span v-else > {{ formattedBranchAndCommit }} </span> -->
  <span v-else v-html="formattedBranchAndCommit(branch, commitNum)"></span>
</div>

</template>

<script setup name="operationDetails">
import { nextTick, ref, onMounted, toRaw,reactive, computed } from 'vue'
const { proxy } = getCurrentInstance();

watchEffect(() => {
    console.log('onMounted nextTick ')
})

const dialogVisible = ref(false);
const selectedOption = ref("")
const options = [
  { value: 'option1', label: '选项1' },
  { value: 'option2', label: '选项2' },
  { value: 'option3', label: '选项3' },
]

const branch = ref("main")
const commitNum = ref("af123456789")
const codeInfoDetail = ref("测试123 \n 测试456")

// const formattedBranchAndCommit = computed(() => {
//   const combinedString = `${branch.value} ${commitNum.value}`;
//   const maxLength = 13;

//   if (combinedString.length > maxLength) {
//     return combinedString.substr(0, maxLength) + '...';
//   }

//   return combinedString;
// });

function formattedBranchAndCommit(branch, commitNum) {
  const combinedString = `${branch} ${commitNum}`;
  const maxLength = 13;

  if (combinedString.length > maxLength) {
    const truncatedCommitNum = commitNum.toString().substr(0, maxLength - branch.length - 1) + '...';
    return `${truncatedCommitNum}`;
  }

  return combinedString;
}

onMounted(() => {
    // Vue.js 提供的一个延迟执行机制，可以在 DOM 更新完成后执行回调函数
    nextTick(() => {
      console.log('onMounted nextTick ')
    })
})

function openDialog() {
  // 打开弹框时设置默认值为第一个选项
  selectedOption.value = options[0].value;
  dialogVisible.value  = true;
}
function handleClose() {
  console.log('弹框关闭');
}
function handleConfirm() {
  console.log('选中的选项是：', this.selectedOption);
  dialogVisible.value = false;
}

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

.commit-num-style {
  background-color: blue;
  color: white;
  padding: 2px 4px;
  border-radius: 3px;
}

</style>
