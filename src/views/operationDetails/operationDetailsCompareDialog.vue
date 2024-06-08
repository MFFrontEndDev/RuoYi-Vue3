<template>

    <el-dialog
          element-loading-text="拼命加载中"
          element-loading-spinner="el-icon-loading"
          element-loading-background="rgba(0, 0, 0, 0.8)"
          :title="leftVersionForm.actionName"
          v-model="operationDetailsCompareDialog"
          :close-on-click-modal="false"
          :before-close="onDialogClose"
          width="90%"
          left
        >
      <div class="page-container">
        <!-- 左侧部分 "-->
        <div class="left-part">
          <div class="contentBox">
              <!-- 左侧内容 -->
              <div class="app-container">
                <el-form
                    ref="leftFormRef"
                    :model="leftVersionForm"
                    :rules="rules"
                    label-width="200"
                    class="demo-ruleForm"
                    status-icon
                >
                    <!-- 基本信息 -->
                    <div class="contentBox topContent">
                        <h3 class="contentTitle">
                            <span class="blueIdentify">基本信息</span>
                            <el-icon class="settingFormBtn" v-if="topContentShowFlag" @click="topContentShowFlag = false" size="20"><ArrowDown /></el-icon>
                            <el-icon class="settingFormBtn" v-else @click="topContentShowFlag = true" size="20"><ArrowRight /></el-icon>
                        </h3>
                        <div v-show="topContentShowFlag" class="formContent">
                            <el-row :gutter="1">
                                <el-col :span="6">
                                    <el-form-item label="操作名称" prop="actionName">
                                      <span style="white-space: nowrap">{{ leftVersionForm.actionName }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="操作Code" prop="actionCode">
                                      <span>{{ leftVersionForm.actionCode }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="操作分类" prop="actionClassDes">
                                      <span>{{ leftVersionForm.actionClassDes }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="操作类型" prop="actionTypeDes">
                                      <span>{{ leftVersionForm.actionTypeDes }}</span>
                                    </el-form-item>
                                </el-col>
                            </el-row>
                            
                            <el-row :gutter="1">
                                <el-col :span="6">
                                    <el-form-item label="状态" prop="actionStatusDes">
                                      <span>{{ "V" + leftVersionForm.actionVersion + "-" + leftVersionForm.actionStatusDes }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="创建人" prop="createByName">
                                      <span>{{ leftVersionForm.createByName }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="创建时间" prop="createTime">
                                      <span style="white-space: nowrap">{{ leftVersionForm.createTime }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="修改人" prop="updateByName">
                                      <span>{{ leftVersionForm.updateByName }}</span>
                                    </el-form-item>
                                </el-col>
                            </el-row>

                            <el-row :gutter="1">
                                <el-col :span="6">
                                    <el-form-item label="修改时间" prop="updateTime">
                                      <span style="white-space: nowrap">{{ leftVersionForm.updateTime }}</span>
                                    </el-form-item>
                                </el-col>
                                <!-- 操作场景字段去除了 
                                <el-col :span="6">
                                    <el-form-item label="操作场景" prop="operationScenario">
                                      <div>
                                          <el-tag v-for="(scenario, index) in leftVersionForm.operationScenario" :key="index" type="info" style="margin-right: 10px; margin-bottom: 5px; background-color: #f0f9eb; color: #409eff; border: 1px solid #c1e2ff">{{ scenario }}</el-tag>
                                      </div>
                                    </el-form-item>
                                </el-col> -->
                                <el-col :span="6">
                                    <el-form-item label="来源" prop="createSourceDes">
                                      <span>{{ leftVersionForm.createSourceDes }}</span>
                                    </el-form-item>
                                </el-col>
                            </el-row>

                            <el-row :gutter="1">
                                <el-col :span="24">
                                    <el-form-item label="操作描述" prop="actionDesc">
                                        <el-input type="textarea" v-model="leftVersionForm.actionDesc" placeholder="请输入操作描述" :disabled="true" show-word-limit maxlength="1000" />
                                    </el-form-item>
                                </el-col>
                            </el-row>
                        </div>
                    </div> 
                    <!-- 执行策略 -->
                    <div class="contentBox middleContent">
                        <h3 class="contentTitle">
                            <span class="blueIdentify">执行策略</span>
                            <el-icon class="settingFormBtn" v-if="middleContentShowFlag" @click="middleContentShowFlag = false" size="20"><ArrowDown /></el-icon>
                            <el-icon class="settingFormBtn" v-else @click="middleContentShowFlag = true" size="20"><ArrowRight /></el-icon>
                        </h3>
                        <div v-show="middleContentShowFlag" class="formContent">
                            <el-row :gutter="1">
                                <el-col :span="6">
                                    <el-form-item label="执行方式" prop="execTypeDes">
                                      <span>{{ leftVersionForm.execTypeDes }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="对象类型" prop="execObjectDes">
                                      <span>{{ leftVersionForm.execObjectDes }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="执行账户" prop="execAccount">
                                      <span>{{ leftVersionForm.execAccount }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="超时时长" prop="execTimeout">
                                      <span>{{ leftVersionForm.execTimeout }}</span>
                                    </el-form-item>
                                </el-col>
                            </el-row>

                            <el-row :gutter="1">
                                <el-col :span="6">
                                    <el-form-item label="超时策略" prop="execTimeoutStrategy">
                                      <span>{{ leftVersionForm.execTimeoutStrategy }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="执行顺序" prop="execOrderDes">
                                      <span>{{ leftVersionForm.execOrderDes }}</span>
                                    </el-form-item>
                                </el-col>
                            </el-row>
                        </div>
                    </div> 
                    <!-- 脚本工程 -->
                    <div class="contentBox bottomContent">
                        <h3 class="contentTitle">
                            <span class="blueIdentify">脚本工程</span>
                            <el-icon class="settingFormBtn" v-if="bottomContentShowFlag" @click="bottomContentShowFlag = false" size="20"><ArrowDown /></el-icon>
                            <el-icon class="settingFormBtn" v-else @click="bottomContentShowFlag = true" size="20"><ArrowRight /></el-icon>
                        </h3>
                    </div> 
                </el-form>
              </div>
          </div>
        </div>

        <!-- 中间分隔线 -->
        <div class="vertical-line"></div>

        <!-- 右侧部分 -->
        <div class="right-part">
          <div class="contentBox">
            <!-- 右侧内容 -->
            <div class="app-container">
                <el-form
                    ref="rightFormRef"
                    :model="rightVersionForm"
                    :rules="rules"
                    label-width="200"
                    class="demo-ruleForm"
                    status-icon
                >
                    <!-- 基本信息 -->
                    <div class="contentBox topContent">
                        <h3 class="contentTitle">
                            <span class="blueIdentify">基本信息</span>
                            <el-icon class="settingFormBtn" v-if="topContentShowFlag" @click="topContentShowFlag = false" size="20"><ArrowDown /></el-icon>
                            <el-icon class="settingFormBtn" v-else @click="topContentShowFlag = true" size="20"><ArrowRight /></el-icon>
                        </h3>
                        <div v-show="topContentShowFlag" class="formContent">
                            <el-row :gutter="1">
                                <el-col :span="6">
                                    <el-form-item label="操作名称" prop="actionName">
                                      <span style="white-space: nowrap">{{ rightVersionForm.actionName }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="操作Code" prop="actionCode">
                                      <span>{{ rightVersionForm.actionCode }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="操作分类" prop="actionClassDes">
                                      <span>{{ rightVersionForm.actionClassDes }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="操作类型" prop="actionTypeDes">
                                      <span>{{ rightVersionForm.actionTypeDes }}</span>
                                    </el-form-item>
                                </el-col>
                            </el-row>
                            
                            <el-row :gutter="1">
                                <el-col :span="6">
                                    <el-form-item label="状态" prop="actionStatusDes">
                                      <span>{{ "V" + rightVersionForm.actionVersion + "-" + leftVersionForm.actionStatusDes }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="创建人" prop="createByName">
                                      <span>{{ rightVersionForm.createByName }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="创建时间" prop="createTime">
                                      <span style="white-space: nowrap">{{ rightVersionForm.createTime }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="修改人" prop="updateByName">
                                      <span>{{ rightVersionForm.updateByName }}</span>
                                    </el-form-item>
                                </el-col>
                            </el-row>

                            <el-row :gutter="1">
                                <el-col :span="6">
                                    <el-form-item label="修改时间" prop="updateTime">
                                      <span style="white-space: nowrap">{{ rightVersionForm.updateTime }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="来源" prop="createSourceDes">
                                      <span>{{ rightVersionForm.createSourceDes }}</span>
                                    </el-form-item>
                                </el-col>
                            </el-row>

                            <el-row :gutter="1">
                                <el-col :span="24">
                                    <el-form-item label="操作描述" prop="actionDesc">
                                        <el-input type="textarea" v-model="rightVersionForm.actionDesc" placeholder="请输入操作描述" :disabled="true" show-word-limit maxlength="1000" />
                                    </el-form-item>
                                </el-col>
                            </el-row>
                        </div>
                    </div> 
                    <!-- 执行策略 -->
                    <div class="contentBox middleContent">
                        <h3 class="contentTitle">
                            <span class="blueIdentify">执行策略</span>
                            <el-icon class="settingFormBtn" v-if="middleContentShowFlag" @click="middleContentShowFlag = false" size="20"><ArrowDown /></el-icon>
                            <el-icon class="settingFormBtn" v-else @click="middleContentShowFlag = true" size="20"><ArrowRight /></el-icon>
                        </h3>
                        <div v-show="middleContentShowFlag" class="formContent">
                            <el-row :gutter="1">
                                <el-col :span="6">
                                    <el-form-item label="执行方式" prop="execTypeDes">
                                      <span>{{ rightVersionForm.execTypeDes }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="对象类型" prop="execObjectDes">
                                      <span>{{ rightVersionForm.execObjectDes }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="执行账户" prop="execAccount">
                                      <span>{{ rightVersionForm.execAccount }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="超时时长" prop="execTimeout">
                                      <span>{{ rightVersionForm.execTimeout }}</span>
                                    </el-form-item>
                                </el-col>
                            </el-row>

                            <el-row :gutter="1">
                                <el-col :span="6">
                                    <el-form-item label="超时策略" prop="execTimeoutStrategy">
                                      <span>{{ rightVersionForm.execTimeoutStrategy }}</span>
                                    </el-form-item>
                                </el-col>
                                <el-col :span="6">
                                    <el-form-item label="执行顺序" prop="execOrderDes">
                                      <span>{{ rightVersionForm.execOrderDes }}</span>
                                    </el-form-item>
                                </el-col>
                            </el-row>
                        </div>
                    </div> 
                    <!-- 脚本工程 -->
                    <div class="contentBox bottomContent">
                        <h3 class="contentTitle">
                            <span class="blueIdentify">脚本工程</span>
                            <el-icon class="settingFormBtn" v-if="bottomContentShowFlag" @click="bottomContentShowFlag = false" size="20"><ArrowDown /></el-icon>
                            <el-icon class="settingFormBtn" v-else @click="bottomContentShowFlag = true" size="20"><ArrowRight /></el-icon>
                        </h3>
                    </div> 
                </el-form>
              </div>
          </div>
        </div>
      </div>
    </el-dialog>
  
</template>

<script setup name="operationDetailsCompareDialog">
import { ref, reactive, getCurrentInstance, defineExpose, defineEmits, toRaw, toRefs, nextTick } from 'vue'
import { ElMessage } from 'element-plus'
import {
    getCompare2VerInfo
} from '../../api/operationDetails/operationDetails'

const { proxy } = getCurrentInstance()
const emit = defineEmits(['showDialog', 'getTabsData'])

const operationDetailsCompareDialog = ref(false)

const topContentShowFlag = ref(true)
const middleContentShowFlag = ref(true)
const bottomContentShowFlag = ref(true)

const leftFormRef = ref(null)
const leftVersionForm = reactive({
    actionName: '',
    actionClass: '',
    actionClassDes: '',
    actionType: '',
    actionTypeDes: '',
    actionVersion: '',
    actionStatus: '',
    createBy: '',
    createByName: '',
    updateBy: '',
    updateByName: '',
    updateTime: '',
    operationScenario: [],
    createSource: '',
    createSourceDes: '',
    actionDesc: '',
    execType: '',
    execTypeDes: '',
    execObject: '',
    execObjectDes: '',
    execAccount: '',
    execTimeout: '',
    execTimeoutStrategy: '',
    execOrder: '',
    execTimeout: ''
})


const rightFormRef = ref(null)
const rightVersionForm = reactive({
    actionName: '',
    actionClass: '',
    actionClassDes: '',
    actionType: '',
    actionTypeDes: '',
    actionVersion: '',
    actionStatus: '',
    createBy: '',
    createByName: '',
    updateBy: '',
    updateByName: '',
    updateTime: '',
    operationScenario: [],
    createSource: '',
    createSourceDes: '',
    actionDesc: '',
    execType: '',
    execTypeDes: '',
    execObject: '',
    execObjectDes: '',
    execAccount: '',
    execTimeout: '',
    execTimeoutStrategy: '',
    execOrder: '',
    execTimeout: ''
})


function showDialog(query) {
    console.log('getCompare2VerInfo 接口入参 query', query)
    getCompare2VerInfo(query).then((response) => {
        const leftVersionKey = 'V'+query.left_version
        const rightVersionKey = 'V'+query.right_version
        if (response.code == 0 || response.code == 200 || response.code == '00000') {
          const leftVersionData = response.data[leftVersionKey];
          const rightVersionData = response.data[rightVersionKey];

          // 左边模块赋值
          leftVersionForm.actionName = leftVersionData.actionName;
          leftVersionForm.actionCode = leftVersionData.actionCode;
          leftVersionForm.actionClass = leftVersionData.actionClass;
          leftVersionForm.actionClassDes = leftVersionData.actionClassDes
          leftVersionForm.actionType = leftVersionData.actionType;
          leftVersionForm.actionTypeDes = leftVersionData.actionTypeDes
          leftVersionForm.actionVersion = leftVersionData.actionVersion;
          leftVersionForm.actionStatus = leftVersionData.actionStatus;
          leftVersionForm.actionStatusDes = leftVersionData.actionStatusDes;
          leftVersionForm.createBy = leftVersionData.createBy;
          leftVersionForm.createByName = leftVersionData.createByName;
          leftVersionForm.createTime = leftVersionData.createTime;
          leftVersionForm.updateBy = leftVersionData.updateBy;
          leftVersionForm.updateByName = leftVersionData.updateByName;
          leftVersionForm.updateTime = leftVersionData.updateTime;
          leftVersionForm.operationScenario = leftVersionData.operationScenario;
          leftVersionForm.createSource = leftVersionData.createSource;
          leftVersionForm.createSourceDes = leftVersionData.createSourceDes;
          leftVersionForm.actionDesc = leftVersionData.actionDesc;
          leftVersionForm.execType = leftVersionData.execType;
          leftVersionForm.execTypeDes = leftVersionData.execTypeDes;
          leftVersionForm.execObject = leftVersionData.execObject;
          leftVersionForm.execObjectDes = leftVersionData.execObjectDes;
          leftVersionForm.execAccount = leftVersionData.execAccount;
          leftVersionForm.execTimeout = leftVersionData.execTimeout;
          leftVersionForm.execTimeoutStrategy = leftVersionData.execTimeoutStrategyDes;
          leftVersionForm.execOrder = leftVersionData.execOrder;
          leftVersionForm.execOrderDes = leftVersionData.execOrderDes;

          // 右边模块赋值
          rightVersionForm.actionName = rightVersionData.actionName;
          rightVersionForm.actionCode = rightVersionData.actionCode;
          rightVersionForm.actionClass = rightVersionData.actionClass;
          rightVersionForm.actionClassDes = rightVersionData.actionClassDes
          rightVersionForm.actionType = rightVersionData.actionType;
          rightVersionForm.actionTypeDes = rightVersionData.actionTypeDes
          rightVersionForm.actionVersion = rightVersionData.actionVersion;
          rightVersionForm.actionStatus = rightVersionData.actionStatus;
          rightVersionForm.actionStatusDes = rightVersionData.actionStatusDes;
          rightVersionForm.createBy = rightVersionData.createBy;
          rightVersionForm.createByName = rightVersionData.createByName;
          rightVersionForm.createTime = rightVersionData.createTime;
          rightVersionForm.updateBy = rightVersionData.updateBy;
          rightVersionForm.updateByName = rightVersionData.updateByName;
          rightVersionForm.updateTime = rightVersionData.updateTime;
          rightVersionForm.operationScenario = rightVersionData.operationScenario;
          rightVersionForm.createSource = rightVersionData.createSource;
          rightVersionForm.createSourceDes = rightVersionData.createSourceDes;
          rightVersionForm.actionDesc = rightVersionData.actionDesc;
          rightVersionForm.execType = rightVersionData.execType;
          rightVersionForm.execTypeDes = rightVersionData.execTypeDes;
          rightVersionForm.execObject = rightVersionData.execObject;
          rightVersionForm.execObjectDes = rightVersionData.execObjectDes;
          rightVersionForm.execAccount = rightVersionData.execAccount;
          rightVersionForm.execTimeout = rightVersionData.execTimeout;
          rightVersionForm.execTimeoutStrategy = rightVersionData.execTimeoutStrategyDes;
          rightVersionForm.execOrder = rightVersionData.execOrder;
          rightVersionForm.execOrderDes = rightVersionData.execOrderDes;
          
        }
    })
    operationDetailsCompareDialog.value = true

}

function beforeClose() {
    // proxy.resetForm('updateConfigTaskFormRef')
    // emit('getTabsData')
    operationDetailsCompareDialog.value = false
    loading.value = false
}


defineExpose({
    showDialog,
    beforeClose
})

</script>

<style scoped>
.inline-block {
  display:inline-block;
}
.display-block{
  display: block !important;
}

.page-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.left-part, .right-part {
  width: 48%; /* 调整左右部分的宽度 */
}

.vertical-line {
  width: 2px;
  height: 100%;
  background-color: #ccc; /* 设置分隔线样式 */
}

</style>

<style>
.tooltip-style {
  width: 300px;
}
.el-popover{
  height: 300px;
  overflow: auto;
}
</style>