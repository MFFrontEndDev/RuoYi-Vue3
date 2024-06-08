<template>
<div class="app-container">
  <el-tabs v-model="tabName" @tab-click="tabClick">
    <el-tab-pane name="操作详情" label="操作详情">
        <div class="button-row">
          <!-- 
          <el-button type="success" plain @click="handleTest">测试</el-button> 
          -->
          <el-button type="warning" plain @click="handleExecute">执行</el-button>
          <el-button type="success" plain @click="handleEdit">编辑</el-button>
          <el-button type="success" plain @click="handleCloneUpdate">克隆更新操作</el-button>
          <el-button type="success" plain @click="handleJobHistory">作业历史</el-button>
          <el-button type="info" plain @click="handleApplyOffline">申请下线</el-button>
          <el-button type="info" plain @click="handleApprovalProcess">审批流程</el-button>
          <el-button type="success" plain @click="handleApprove">通过</el-button>
          <el-button type="danger" plain @click="handleReject">拒绝</el-button>
         
          <el-select
            v-model="compareLeftVersionValue"
            clearable
            placeholder="请选择"
            style="width: 120px"
          >
            <el-option
              v-for="item in compareLeftVersionOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value"
              :disabled="compareRightVersionValue === item.value"
            />
          </el-select>

          <el-button type="text" class="vs-button" @click="openCompareDialog"></el-button>
          

          <el-select
            v-model="compareRightVersionValue"
            clearable
            placeholder="⇋版本对比"
            style="width: 120px"
          >
            <el-option
              v-for="item in compareRightVersionOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value"
              :disabled="compareLeftVersionValue === item.value"
            />
          </el-select>
        </div>
        
        <div class="app-container">
            <el-form
                ref="fileFormRef"
                :model="ruleForm"
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
                                  <span :title="ruleForm.actionName" class="truncate-text">{{ ruleForm.actionName }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="操作Code" prop="actionCode">
                                  <span :title="ruleForm.actionCode" class="truncate-text">{{ ruleForm.actionCode }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="操作分类" prop="actionClassDes">
                                  <span :title="ruleForm.actionClassDes" class="truncate-text">{{ ruleForm.actionClassDes }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="操作类型" prop="actionTypeDes">
                                  <span :title="ruleForm.actionTypeDes" class="truncate-text">{{ ruleForm.actionTypeDes }}</span>
                                </el-form-item>
                            </el-col>
                        </el-row>
                        
                        <el-row :gutter="1">
                            <el-col :span="6">
                                <el-form-item label="状态" prop="actionStatusDes">
                                  <span :title="ruleForm.actionStatusDes" class="truncate-text">{{ "V" + ruleForm.actionVersion + "-" + ruleForm.actionStatusDes }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="创建人" prop="createByName">
                                  <!-- <span>{{ ruleForm.createByName }}</span> -->
                                  <span :title="ruleForm.createByName" class="truncate-text">{{ ruleForm.createByName }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="创建时间" prop="createTime">
                                  <span :title="ruleForm.createTime" class="truncate-text">{{ ruleForm.createTime }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="修改人" prop="updateByName">
                                  <span :title="ruleForm.updateByName" class="truncate-text">{{ ruleForm.updateByName }}</span>
                                </el-form-item>
                            </el-col>
                        </el-row>

                        <el-row :gutter="1">
                            <el-col :span="6">
                                <el-form-item label="修改时间" prop="updateTime">
                                  <span :title="ruleForm.updateTime" class="truncate-text">{{ ruleForm.updateTime }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="来源" prop="createSourceDes">
                                  <span :title="ruleForm.createSourceDes" class="truncate-text">{{ ruleForm.createSourceDes }}</span>
                                </el-form-item>
                            </el-col>
                        </el-row>

                        <el-row :gutter="1">
                            <el-col :span="24">
                                <el-form-item label="操作描述" prop="actionDesc">
                                    <el-input type="textarea" v-model="ruleForm.actionDesc" placeholder="请输入操作描述" :disabled="true" show-word-limit maxlength="1000" />
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
                                  <span :title="ruleForm.execTypeDes" class="truncate-text">{{ ruleForm.execTypeDes }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="对象类型" prop="execObjectDes">
                                  <span :title="ruleForm.execObjectDes" class="truncate-text">{{ ruleForm.execObjectDes }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="执行账户" prop="execAccount">
                                  <span :title="ruleForm.execAccount" class="truncate-text">{{ ruleForm.execAccount }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="超时时长" prop="execTimeout">
                                  <span :title="ruleForm.execTimeout" class="truncate-text">{{ ruleForm.execTimeout }}</span>
                                </el-form-item>
                            </el-col>
                        </el-row>

                        <el-row :gutter="1">
                            <el-col :span="6">
                                <el-form-item label="超时策略" prop="execTimeoutStrategy">
                                  <span :title="ruleForm.execTimeoutStrategy" class="truncate-text">{{ ruleForm.execTimeoutStrategy }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="执行顺序" prop="execOrderDes">
                                  <span :title="ruleForm.execOrderDes" class="truncate-text">{{ ruleForm.execOrderDes }}</span>
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
    </el-tab-pane>

    <el-tab-pane name="活动记录" label="活动记录">
      <el-table
          ref="paramTableRef"
          :data="activityRecordTableData"
          stripe
          row-key="id"
          style="width: 100%"
      >
        <el-table-column label="事件类型" align="center"  prop="actionStatus" >
          <template #default="scope">
            <el-tooltip v-if="scope.row.actionStatus.length>20"
                        placement="top"
                        popper-class="tooltip-width"
                        :content="scope.row.actionStatus">
              <span>{{ scope.row.actionStatus.length>20?scope.row.actionStatus.toString().substr(0,20)+'...':scope.row.actionStatus }}</span>
            </el-tooltip>
            <span v-else>{{ scope.row.actionStatus }}</span>
          </template>
        </el-table-column>
        <el-table-column label="操作人" align="center" prop="modifyUser" width="180">
          <template #default="scope">
            <div class="modifier-style">
              <div class="modifier-text">
                {{ scope.row.modifyUser.userName + '-' + scope.row.modifyUser.nickName }}
              </div>
            </div>
          </template>
        </el-table-column>
        <el-table-column label="时间" align="center" prop="modifyTime" width="180" />
        <el-table-column label="备注" align="center" prop="actionDesc" width="300">
          <template #default="scope">
            <el-tooltip v-if="scope.row.actionDesc && scope.row.actionDesc.length>100"
                        placement="top"
                        popper-class="tooltip-width"
                        :content="scope.row.actionDesc">
              <span>{{ scope.row.actionDesc.length>100?scope.row.actionDesc.toString().substr(0,100)+'...':scope.row.actionDesc }}</span>
            </el-tooltip>
            <span v-else>{{ scope.row.actionDesc }}</span>
          </template>
        </el-table-column>     
      </el-table>
      <pagination
            v-show="queryPage.total > 0"
            :total="queryPage.total"
            v-model:page="queryPage.pageNum"
            v-model:limit="queryPage.pageSize"
            @pagination="pendingTasks"
      />
    </el-tab-pane>
  </el-tabs>
</div>

<operationDetailsCompareDialog ref="operationDetailsCompareDialogRef" @getTabsData="getTabsData"></operationDetailsCompareDialog>

</template>

<script setup name="operationDetails">
import { nextTick, ref, onMounted, toRaw,reactive } from 'vue'
import { EditPen, Delete, MoreFilled, View, Hide } from '@element-plus/icons-vue'
import useUserStore from '@/store/modules/user'

import operationDetailsCompareDialog from './operationDetailsCompareDialog'

import {
    getActivityRecordList,
    getActionVersionList,
    getActionDetailInfo
} from '../../api/operationDetails/operationDetails'
import { getNodeTableData } from '@/api/scriptManagementPlatform/operationManage'

import { ElMessage, ElMessageBox } from 'element-plus'

const fileFormRef = ref(null)
const topContentShowFlag = ref(true)
const middleContentShowFlag = ref(true)
const bottomContentShowFlag = ref(true)
const { proxy } = getCurrentInstance()

// const compareDialogVisible = ref(false)
const operationDetailsCompareDialogRef = ref(null)

// 操作列表页面传递来的详情对象
// rowData 定义为一个响应式对象
const rowData = reactive({
    id: null,
    actionName: '',
    actionClassList: [],
    actionCode: '',
    actionType: '',
    actionUser: [],
    actionDesc: '',
    execType: '',
    execObject: '',
    execAccount: '',
    execTimeout: '',
    execTimeoutStrategy: '',
    execOrder: '',
    actionStatus: ''
})

const userStore = useUserStore()
const loading = ref(false)
const tabName = ref('操作详情')
const tableData = ref([])
const queryPage = reactive({
    total: 100,
    pageNum: 1,
    pageSize: 10,
    actionCode: null
})

// 下拉框数据源
const compareLeftVersionValue = ref(null)
const compareLeftVersionOptions = ref([]); // 使用 ref() 创建响应式变量
const compareRightVersionValue = ref(null)
const compareRightVersionOptions = ref([]);

const ruleForm = reactive({
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

const imgUrl = ref('')
function tabClick(tab, event) {
    const tabRaw = toRaw(tab)
    tabName.value = toRaw(tabRaw.paneName.value)
    if (toRaw(tabName.value) == '操作详情') {
      // todo 放开注释
        // getActionVersionListData()
        // getActionDetailInfoData()
        // todo 测试数据 待处理
        getActionDetailInfoDataTest()
    }
    if (toRaw(tabName.value) == '活动记录') {
        // getActivityRecordData() todo 
        getActivityRecordDataTest()
    }
}

const activityRecordTableData = ref([])

watchEffect(() => {
    // console.log('rowData.actionCode: ', rowData.actionCode)
    // if (rowData.actionCode) {
    //     getActivityRecordData()
    // }
})

// 查询活动历史记录
function getActivityRecordData() { 
    // 'ceshi1'
    console.log('rowData.actionCode： ',rowData.actionCode)
    queryPage.actionCode = rowData.actionCode
    const paramsData = {
        pageNum: queryPage.pageNum,
        pageSize: queryPage.pageSize,
        action_code: queryPage.actionCode
    }
    console.log('getActivityRecordData 开始执行，入参: ', paramsData);
    getActivityRecordList(paramsData).then((response) => {
        console.log('getActivityRecordList 返回结果: ', response);
        queryPage.total = 0
        if (response.code == 0 || response.code == 200 || response.code == '00000') {
            const data = response.rows
            for (let i = 0; i < data.length; i++) {
                if (data[i].actionStatus == 10) {
                    data[i].actionStatus = '开发中'
                } else if (data[i].actionStatus == 15) {
                    data[i].actionStatus = '发布待审核'
                } else if (data[i].actionStatus == 20) {
                    data[i].actionStatus = '已发布'
                } else if (data[i].actionStatus == -15) {
                    data[i].actionStatus = '下线待审核'
                } else if (data[i].actionStatus == -10) {
                    data[i].actionStatus = '已拒绝'
                } else if (data[i].actionStatus == -20) {
                    data[i].actionStatus = '已下线'
                } else if (data[i].actionStatus == -1) {
                    data[i].actionStatus = '已删除'
                }
            }
            activityRecordTableData.value = data
            debugger
            queryPage.total = response.total
        }
    })
}

function getActivityRecordDataTest() {
    const mockData = [
        {"actionStatus":10,"modifyUser":{"userName":"013597","nickName":"changyou"},"modifyTime":"2024-05-20 15:49:18","actionDesc":"备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息备注信息"},
        {"actionStatus":15,"modifyUser":{"userName":"013597","nickName":"changyou"},"modifyTime":"2024-05-20 15:49:18","actionDesc":"备注信息"},
        {"actionStatus":20,"modifyUser":{"userName":"013597","nickName":"changyou"},"modifyTime":"2024-05-20 15:49:18","actionDesc":"备注信息"},
        {"actionStatus":-15,"modifyUser":{"userName":"013597","nickName":"changyou"},"modifyTime":"2024-05-20 15:49:18","actionDesc":"备注信息"},
        {"actionStatus":-10,"modifyUser":{"userName":"013597","nickName":"changyou"},"modifyTime":"2024-05-20 15:49:18","actionDesc":"备注信息"},
        {"actionStatus":-20,"modifyUser":{"userName":"013597","nickName":"changyou"},"modifyTime":"2024-05-20 15:49:18","actionDesc":"备注信息"},
        {"actionStatus":-1,"modifyUser":{"userName":"013597","nickName":"changyou"},"modifyTime":"2024-05-20 15:49:18","actionDesc":"备注信息"},
        {"actionStatus":10,"modifyUser":{"userName":"013597","nickName":"changyou"},"modifyTime":"2024-05-20 15:49:18","actionDesc":"备注信息"},
        {"actionStatus":10,"modifyUser":{"userName":"013597","nickName":"changyou"},"modifyTime":"2024-05-20 15:49:18","actionDesc":"备注信息"},
        {"actionStatus":10,"modifyUser":{"userName":"013597","nickName":"changyou"},"modifyTime":"2024-05-20 15:49:18","actionDesc":"备注信息"},
        {"actionStatus":10,"modifyUser":{"userName":"013597","nickName":"changyou"},"modifyTime":"2024-05-20 15:49:18","actionDesc":"备注信息"}
    ];

    const data = mockData.map(item => {
        switch (item.actionStatus) {
            case 10:
                item.actionStatus = '开发中';
                break;
            case 15:
                item.actionStatus = '发布待审核';
                break;
            case 20:
                item.actionStatus = '已发布';
                break;
            case -15:
                item.actionStatus = '下线待审核';
                break;
            case -10:
                item.actionStatus = '已拒绝';
                break;
            case -20:
                item.actionStatus = '已下线';
                break;
            case -1:
                item.actionStatus = '已删除';
                break;
            default:
                break;
        }
        return item;
    });

    activityRecordTableData.value = data;
}

// 查询可选版本下拉列表
function getActionVersionListData() {
    // 'ceshi1'
    queryPage.actionCode = rowData.actionCode
    const paramsData = {
        action_code: queryPage.actionCode
    }
    console.log('getActionVersionList 开始执行，入参: ', paramsData);
    getActionVersionList(paramsData).then((response) => {
        console.log('getActionVersionList 返回结果: ', response);
        if (response.code == 0 || response.code == 200 || response.code == '00000') {
            const data = response.data
            const options = data.map(item => {
                return {
                    label: 'v' + item.actionVersion + '(' + item.actionStatusDes + ')',
                    value: item.actionVersion
                };
            });
            console.log('getActionVersionList 返回结果 options: ', options);
            compareLeftVersionOptions.value = options;
            compareRightVersionOptions.value = options;

            // Set the default selected value for compareLeftVersionValue if needed
            if (options.length > 0) {
                compareLeftVersionValue.value = options[0].value;
                compareRightVersionValue.value = options[0].value;
            }
        }
    })
}

// 根据 actionCode 查询操作详情
function getActionDetailInfoData(actionCode) {
    queryPage.actionCode = actionCode
    const paramsData = {
        action_code: queryPage.actionCode
    }
    console.log('getActionDetailInfo 开始执行，入参: ', paramsData);
    getActionDetailInfo(paramsData).then((response) => {
        console.log('getActionDetailInfo 返回结果: ', response);
        if (response.code == 0 || response.code == 200 || response.code == '00000') {
            const data = response.data

            // 将 mockData 中的数据赋值给 ruleForm 对象
            ruleForm.actionName = response.data.actionName;
            ruleForm.actionCode = response.data.actionCode;
            ruleForm.actionClass = response.data.actionClass;
            ruleForm.actionClassDes = response.data.actionClassDes
            ruleForm.actionType = response.data.actionType;
            ruleForm.actionTypeDes = response.data.actionTypeDes
            ruleForm.actionVersion = response.data.actionVersion;
            ruleForm.actionStatus = response.data.actionStatus;
            ruleForm.actionStatusDes = response.data.actionStatusDes;
            ruleForm.createBy = response.data.createBy;
            ruleForm.createByName = response.data.createByName;
            ruleForm.createTime = response.data.createTime;
            ruleForm.updateBy = response.data.updateBy;
            ruleForm.updateByName = response.data.updateByName;
            ruleForm.updateTime = response.data.updateTime;
            ruleForm.operationScenario = response.data.operationScenario;
            ruleForm.createSource = response.data.createSource;
            ruleForm.createSourceDes = response.data.createSourceDes;
            ruleForm.actionDesc = response.data.actionDesc;
            ruleForm.execType = response.data.execType;
            ruleForm.execTypeDes = response.data.execTypeDes;
            ruleForm.execObject = response.data.execObject;
            ruleForm.execObjectDes = response.data.execObjectDes;
            ruleForm.execAccount = response.data.execAccount;
            ruleForm.execTimeout = response.data.execTimeout;
            ruleForm.execTimeoutStrategy = response.data.execTimeoutStrategyDes;
            ruleForm.execOrder = response.data.execOrder;
            ruleForm.execOrderDes = response.data.execOrderDes;
            
        }
    })
}

function getActionDetailInfoDataTest() {
    const mockData = {
        "code": "00000",
        "message": "success",
        "data": {
            "actionName": "应用申请_用户域名_新建",
            "actionCode": "app_userdomain_new",
            "actionClass": 1,
            "actionClassDes": "通用平台-通用",
            "actionType": 1,
            "actionTypeDes": "变更",
            "actionVersion": 2,
            "actionStatus": 10,
            "actionStatusDes": "开发中",
            "createBy": "013597",
            "createByName": "常友",
            "createTime": "2024-01-16 13:00:00",
            "updateBy": "013597",
            "updateByName": "常友",
            "updateTime": "2024-01-18 13:00:00",
            "operationScenario": [
                "安装部署",
                "申请资源",
                "test..."
            ],
            "createSource": 1,
            "createSourceDes": "操作新建",
            "actionDesc": "这里是描述信息...",
            "execType": 1,
            "execTypeDes": "远程代理",
            "execObject": 1,
            "execObjectDes": "主机",
            "execAccount": "执行账户",
            "execTimeout": "5(分钟)",
            "execTimeoutStrategy": "超时等待",
            "execOrder": 2,
            "execOrderDes": "并行"
        }
    }
    
    // 将 mockData 中的数据赋值给 ruleForm 对象
    ruleForm.actionName = mockData.data.actionName;
    ruleForm.actionCode = mockData.data.actionCode;
    ruleForm.actionClass = mockData.data.actionClass;
    ruleForm.actionClassDes = mockData.data.actionClassDes
    ruleForm.actionType = mockData.data.actionType;
    ruleForm.actionTypeDes = mockData.data.actionTypeDes
    ruleForm.actionVersion = mockData.data.actionVersion;
    ruleForm.actionStatus = mockData.data.actionStatus;
    ruleForm.actionStatusDes = mockData.data.actionStatusDes;
    ruleForm.createBy = mockData.data.createBy;
    ruleForm.createByName = mockData.data.createByName;
    ruleForm.createTime = mockData.data.createTime;
    ruleForm.updateBy = mockData.data.updateBy;
    ruleForm.updateByName = mockData.data.updateByName;
    ruleForm.updateTime = mockData.data.updateTime;
    ruleForm.operationScenario = mockData.data.operationScenario;
    ruleForm.createSource = mockData.data.createSource;
    ruleForm.createSourceDes = mockData.data.createSourceDes;
    ruleForm.actionDesc = mockData.data.actionDesc;
    ruleForm.execType = mockData.data.execType;
    ruleForm.execTypeDes = mockData.data.execTypeDes;
    ruleForm.execObject = mockData.data.execObject;
    ruleForm.execObjectDes = mockData.data.execObjectDes;
    ruleForm.execAccount = mockData.data.execAccount;
    ruleForm.execTimeout = mockData.data.execTimeout;
    ruleForm.execTimeoutStrategy = mockData.data.execTimeoutStrategy;
    ruleForm.execOrder = mockData.data.execOrder;
    ruleForm.execOrderDes = mockData.data.execOrderDes;

}

function openCompareDialog() {
  if (!compareLeftVersionValue.value || !compareRightVersionValue.value) {
    ElMessageBox.alert('请选择需要对比的版本号！');
    return;
  }
  if (compareLeftVersionValue.value == compareRightVersionValue.value) {
    ElMessageBox.alert('请选择不同的版本号！');
    return;
  }
  const leftVersion = compareLeftVersionValue.value
  const rightVersion = compareRightVersionValue.value
  const query = {
    'left_version':leftVersion,
    'right_version':rightVersion,
    'action_code':rowData.actionCode
  }
  operationDetailsCompareDialogRef.value.showDialog(toRaw(query))
  
}

function showPassword(row) {
    row.password = row.defaultValue
}
function hidePassword(row) {
    row.password = null
}

onMounted(() => {
    // Vue.js 提供的一个延迟执行机制，可以在 DOM 更新完成后执行回调函数
    nextTick(() => {
        if (proxy.$route.query && proxy.$route.query.rowId) {
            console.log('proxy.$route.query.rowId',proxy.$route.query.rowId)
            const queryId = proxy.$route.query.rowId
            getEditFormData(queryId)            
        } else {
            rowData  = null
        }
    })
})

function getEditFormData(id) {
    console.log('getEditFormData 入参 id: ',id)
    getNodeTableData({ actionMstId: [id] }).then(res => {
        const resultData = res.rows[0]
        const { id, version, actionVersion, actionCodeUneditable, actionName, actionClassList, actionCode, actionType, actionUser, actionDesc, execType, execObject, execAccount, execTimeout, execTimeoutStrategy, execOrder,actionStatus } = resultData
        rowData.id = id
        rowData.actionName = actionName
        rowData.actionClassList = actionClassList.map(num => String(num))
        rowData.actionCode = actionCode
        rowData.actionType = actionType
        rowData.actionUser = actionUser.split(',')
        rowData.actionDesc = actionDesc
        rowData.execType = execType
        rowData.execObject = execObject
        rowData.execAccount = execAccount
        rowData.execTimeout = execTimeout
        rowData.execTimeoutStrategy = execTimeoutStrategy
        rowData.execOrder = execOrder
        rowData.actionStatus = actionStatus
        console.log("rowData.actionCode",rowData.actionCode)
        // 根据actionCode查询详情信息
        getActionDetailInfoData(actionCode)
        // 查询下拉列表对比版本号信息
        getActionVersionListData()
    })
}

function handleTest() {
  // todo 测试按钮点击事件处理逻辑 
  // proxy.$router.push({
  //     path: '/operationDetails/toDo'
  // });
  window.open('/operationDetails/todo', '_blank');
}

function handleExecute() {
  // todo 执行按钮点击事件处理逻辑
  // proxy.$router.push({
  //     path: '/operationDetails/toDo'
  // });
  window.open('/operationDetails/todo', '_blank');
}
function handleEdit() {
  // 编辑按钮点击事件处理逻辑
  const row = rowData
  console.log("编辑按钮点击时候，当前 rowData",row)
  debugger
  // 已发布/已拒绝/已下线/发布待审核/下线待审核
  if (row.actionStatus == '20' || row.actionStatus == '-10' || row.actionStatus == '-20' || row.actionStatus == '15' || row.actionStatus == '-15') {
        proxy.$modal.confirm(
            '您是否要编辑当前选中的版本数据，点击是后将生成一份新的版本！',
            'Warning',
            {
                confirmButtonText: '是',
                cancelButtonText: '否',
                type: 'warning'
            }
        )
            .then(() => {
                proxy.$router.push({
                    path: '/scriptManagementPlatform/operationManage',
                    query: {
                        id: row.id
                    }
                })
            })
            .catch(() => {

            })
    } else {
        proxy.$router.push({
            path: '/scriptManagementPlatform/operationManage',
            query: {
                id: row.id
            }
        })
    }
}

function handleCloneUpdate() {
  // 克隆更新操作按钮点击事件处理逻辑
}
function handleJobHistory() {
  // todo 作业历史按钮点击事件处理逻辑
  // proxy.$router.push({
  //     path: '/operationDetails/toDo'
  // });
  window.open('/operationDetails/todo', '_blank');
}
function handleApplyOffline() {
  // 申请下线按钮点击事件处理逻辑
}
function handleApprovalProcess() {
  // 审批流程按钮点击事件处理逻辑
}
function handleApprove() {
  // 通过按钮点击事件处理逻辑
}
function handleReject() {
  // 拒绝按钮点击事件处理逻辑
}

</script>

<style scoped>
.create-button-style{
  position: absolute;
  right: 20px;
  z-index: 1001;
}
.font-size-18{
  font-size: 18px;
}
.user-avatar{
  cursor: pointer;
  width: 32px;
  height: 32px;
  border-radius: 16px;
  display: inline-block;
}
.ref-pipeline-style{
  display: inline-block;
  text-align: center;
  vertical-align: middle;
}
.ref-pipeline-row-text{
  display: inline-block;
  line-height: 32px;
  vertical-align: middle;
}
.ref-pipeline-popover{
  display: inline-block;
  line-height: 32px;
  vertical-align: middle;
  margin-top: 10px;
}
.more-filled-icon-style{
  display: inline-block;
  cursor: pointer;
  line-height: 32px;
  margin-left: 20px;
}
.ref-pipeline-array-style{
  text-align: center;
  line-height: 32px;
}
.modifier-style{
  display:inline-block !important;
  line-height: 32px !important;
}
.modifier-img-style{
  display:inline-block !important;
  vertical-align: middle;
  margin-top: 10px;
}
.modifier-text{
  display:inline-block;
  vertical-align: middle;
  margin-left: 20px;
}
.cursor-pointer{
  cursor: pointer;
}
.margin-left-20{
  margin-left: 20px
}
.display-flex{
  display: flex;
}
.password-style{
  display: inline-block;
  width: 120px;
  vertical-align: middle;
  margin-top: 1px;
}
.password-button-style{
  display: inline-block;
  vertical-align: middle;
  margin-left: 10px;
}


.button-row {
  /*display: flex;*/
  justify-content: space-around;
  margin-top: 20px;
}

.button-row .el-button {
  color: white;
}

.no-shadow-button {
  box-shadow: none; /* 去除按钮的阴影效果 */
}

.button-row .el-button:nth-child(1) {
  background-color: #F59A23; /* 淡黄色 */
}

.button-row .el-button:nth-child(2),
.button-row .el-button:nth-child(7) {
  background-color: #52c41a; /* 绿色 */
}

.button-row .el-button:nth-child(3),
.button-row .el-button:nth-child(4),
.button-row .el-button:nth-child(5),
.button-row .el-button:nth-child(6) {
  background-color: #409eff; /* 蓝色 */
}

.button-row .el-button:nth-child(8) {
  background-color: #f5222d; /* 红色 */
}

.select-box {
  width: 120px;
}

.vs-button { 
  background-image: url('@/assets/images/TablerVs.svg'); 
  background-size: 30px 30px; /* 调整背景图片大小为长宽60 */
  background-repeat: no-repeat;
  padding: 16px 18px; /* 可以根据需要调整按钮的内边距 */
  color: transparent; /* 隐藏按钮的文字内容 */
}

.settingFormBtn {
    position: relative;
    left: 5px;
    top: 3px;
    cursor: pointer;
}
.formContent {
    border: 1px solid #c5c6c9;
    padding: 20px;
}

.truncate-text {
  display: inline-block;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.truncate-text:hover {
  overflow: visible;
  white-space: normal;
}

.tooltip-width {
  max-width: 300px; /* 调整tooltip宽度，根据实际需要进行调整 */
}

</style>

<style>
.tooltip-width {
  max-width: 300px;
}
</style>