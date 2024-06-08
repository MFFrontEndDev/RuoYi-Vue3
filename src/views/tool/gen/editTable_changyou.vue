<template>
  <el-card>
    <el-tabs v-model="activeName">
      <el-tab-pane label="基本信息" name="basic">
        <basic-info-form ref="basicInfo" :info="info" />
      </el-tab-pane>
      <el-tab-pane label="操作详情" name="columnInfo">

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
                label-width="100"
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
                                  <!-- <span :title="ruleForm.actionName" class="truncate-text">{{ ruleForm.actionName }}</span> -->
                                  <el-tooltip v-if="ruleForm.actionName && ruleForm.actionName.length > 25" placement="top" :content="ruleForm.actionName">
                                    <span :title="ruleForm.actionName" class="truncate-text">{{ ruleForm.actionName.length > 25 ? ruleForm.actionName.substr(0, 25) + '...' : ruleForm.actionName }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.actionName" class="truncate-text">{{ ruleForm.actionName || '--' }}</span>

                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="操作Code" prop="actionCode">
                                  <!-- <span :title="ruleForm.actionCode" class="truncate-text">{{ ruleForm.actionCode }}</span> -->
                                  <el-tooltip v-if="ruleForm.actionCode && ruleForm.actionCode.length > 25" placement="top" :content="ruleForm.actionCode">
                                    <span :title="ruleForm.actionCode" class="truncate-text">{{ ruleForm.actionCode.length > 25 ? ruleForm.actionCode.substr(0, 25) + '...' : ruleForm.actionCode }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.actionCode" class="truncate-text">{{ ruleForm.actionCode || '--' }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="操作分类" prop="actionClassDes">
                                  <!-- <span :title="ruleForm.actionClassDes" class="truncate-text">{{ ruleForm.actionClassDes }}</span> -->
                                  <el-tooltip v-if="ruleForm.actionClassDes && ruleForm.actionClassDes.length > 25" placement="top" :content="ruleForm.actionClassDes">
                                    <span :title="ruleForm.actionClassDes" class="truncate-text">{{ ruleForm.actionClassDes.length > 25 ? ruleForm.actionClassDes.substr(0, 25) + '...' : ruleForm.actionClassDes }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.actionClassDes" class="truncate-text">{{ ruleForm.actionClassDes || '--' }}</span>

                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="操作类型" prop="actionTypeDes">
                                  <!-- <span :title="ruleForm.actionTypeDes" class="truncate-text">{{ ruleForm.actionTypeDes }}</span> -->
                                  <el-tooltip v-if="ruleForm.actionTypeDes && ruleForm.actionTypeDes.length > 25" placement="top" :content="ruleForm.actionTypeDes">
                                    <span :title="ruleForm.actionTypeDes" class="truncate-text">{{ ruleForm.actionTypeDes.length > 25 ? ruleForm.actionTypeDes.substr(0, 25) + '...' : ruleForm.actionTypeDes }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.actionTypeDes" class="truncate-text">{{ ruleForm.actionTypeDes || '--' }}</span>

                                </el-form-item>
                            </el-col>
                        </el-row>
                        
                        <el-row :gutter="1">
                            <el-col :span="6">
                                <el-form-item label="状态" prop="actionStatusDes">
                                  <!-- <span :title="ruleForm.actionStatusDes" class="truncate-text">{{ "V" + ruleForm.actionVersion + "-" + ruleForm.actionStatusDes }}</span> -->
                                  <el-tooltip v-if="ruleForm.actionStatusDes && ruleForm.actionStatusDes.length > 25" placement="top" :content="ruleForm.actionStatusDes">
                                    <span :title="ruleForm.actionStatusDes" class="truncate-text">{{ ruleForm.actionStatusDes.length > 25 ? ruleForm.actionStatusDes.substr(0, 25) + '...' : ruleForm.actionStatusDes }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.actionStatusDes" class="truncate-text">{{ ruleForm.actionStatusDes || '--' }}</span>

                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="创建人" prop="createByName">
                                  <!-- <span :title="ruleForm.createByName" class="truncate-text">{{ ruleForm.createByName }}</span> -->
                                  <el-tooltip v-if="ruleForm.createByName && ruleForm.createByName.length > 25" placement="top" :content="ruleForm.createByName">
                                    <span :title="ruleForm.createByName" class="truncate-text">{{ ruleForm.createByName.length > 25 ? ruleForm.createByName.substr(0, 25) + '...' : ruleForm.createByName }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.createByName" class="truncate-text">{{ ruleForm.createByName || '--' }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="创建时间" prop="createTime">
                                  <!-- <span :title="ruleForm.createTime" class="truncate-text">{{ ruleForm.createTime }}</span> -->
                                  <el-tooltip v-if="ruleForm.createTime && ruleForm.createTime.length > 25" placement="top" :content="ruleForm.createTime">
                                    <span :title="ruleForm.createTime" class="truncate-text">{{ ruleForm.createTime.length > 25 ? ruleForm.createTime.substr(0, 25) + '...' : ruleForm.createTime }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.createTime" class="truncate-text">{{ ruleForm.createTime || '--' }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="修改人" prop="updateByName">
                                  <!-- <span :title="ruleForm.updateByName" class="truncate-text">{{ ruleForm.updateByName }}</span> -->
                                  <el-tooltip v-if="ruleForm.updateByName && ruleForm.updateByName.length > 25" placement="top" :content="ruleForm.updateByName">
                                    <span :title="ruleForm.updateByName" class="truncate-text">{{ ruleForm.updateByName.length > 25 ? ruleForm.updateByName.substr(0, 25) + '...' : ruleForm.updateByName }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.updateByName" class="truncate-text">{{ ruleForm.updateByName || '--' }}</span>
                                </el-form-item>
                            </el-col>
                        </el-row>

                        <el-row :gutter="1">
                            <el-col :span="6">
                                <el-form-item label="修改时间" prop="updateTime">
                                  <!-- <span :title="ruleForm.updateTime" class="truncate-text">{{ ruleForm.updateTime }}</span> -->
                                  <el-tooltip v-if="ruleForm.updateTime && ruleForm.updateTime.length > 25" placement="top" :content="ruleForm.updateTime">
                                    <span :title="ruleForm.updateTime" class="truncate-text">{{ ruleForm.updateTime.length > 25 ? ruleForm.updateTime.substr(0, 25) + '...' : ruleForm.updateTime }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.updateTime" class="truncate-text">{{ ruleForm.updateTime || '--' }}</span>
                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="来源" prop="createSourceDes">
                                  <!-- <span :title="ruleForm.createSourceDes" class="truncate-text">{{ ruleForm.createSourceDes }}</span> -->
                                  <el-tooltip v-if="ruleForm.createSourceDes && ruleForm.createSourceDes.length > 25" placement="top" :content="ruleForm.createSourceDes">
                                    <span :title="ruleForm.createSourceDes" class="truncate-text">{{ ruleForm.createSourceDes.length > 25 ? ruleForm.createSourceDes.substr(0, 25) + '...' : ruleForm.createSourceDes }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.createSourceDes" class="truncate-text">{{ ruleForm.createSourceDes || '--' }}</span>
                                </el-form-item>
                            </el-col>
                        </el-row>

                        <el-row :gutter="1">
                            <el-col :span="16">
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
                                  <!-- <span :title="ruleForm.execTypeDes" class="truncate-text">{{ ruleForm.execTypeDes }}</span> -->
                                  <el-tooltip v-if="ruleForm.execTypeDes && ruleForm.execTypeDes.length > 25" placement="top" :content="ruleForm.execTypeDes">
                                    <span :title="ruleForm.execTypeDes" class="truncate-text">{{ ruleForm.execTypeDes.length > 25 ? ruleForm.execTypeDes.substr(0, 25) + '...' : ruleForm.execTypeDes }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.execTypeDes" class="truncate-text">{{ ruleForm.execTypeDes || '--' }}</span>

                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="对象类型" prop="execObjectDes">
                                  <!-- <span :title="ruleForm.execObjectDes" class="truncate-text">{{ ruleForm.execObjectDes }}</span> -->
                                  <el-tooltip v-if="ruleForm.execObjectDes && ruleForm.execObjectDes.length > 25" placement="top" :content="ruleForm.execObjectDes">
                                    <span :title="ruleForm.execObjectDes" class="truncate-text">{{ ruleForm.execObjectDes.length > 25 ? ruleForm.execObjectDes.substr(0, 25) + '...' : ruleForm.execObjectDes }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.execObjectDes" class="truncate-text">{{ ruleForm.execObjectDes || '--' }}</span>

                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="执行账户" prop="execAccount">
                                  <!-- <span :title="ruleForm.execAccount" class="truncate-text">{{ ruleForm.execAccount }}</span> -->
                                  <el-tooltip v-if="ruleForm.execAccount && ruleForm.execAccount.length > 25" placement="top" :content="ruleForm.execAccount">
                                    <span :title="ruleForm.execAccount" class="truncate-text">{{ ruleForm.execAccount.length > 25 ? ruleForm.execAccount.substr(0, 25) + '...' : ruleForm.execAccount }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.execAccount" class="truncate-text">{{ ruleForm.execAccount || '--' }}</span>

                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="超时时长" prop="execTimeout">
                                  <!-- <span :title="ruleForm.execTimeout" class="truncate-text">{{ ruleForm.execTimeout }}</span> -->
                                  <el-tooltip v-if="ruleForm.execTimeout && ruleForm.execTimeout.length > 25" placement="top" :content="ruleForm.execTimeout">
                                    <span :title="ruleForm.execTimeout" class="truncate-text">{{ ruleForm.execTimeout.length > 25 ? ruleForm.execTimeout.substr(0, 25) + '...' : ruleForm.execTimeout }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.execTimeout" class="truncate-text">{{ ruleForm.execTimeout || '--' }}</span>

                                </el-form-item>
                            </el-col>
                        </el-row>

                        <el-row :gutter="1">
                            <el-col :span="6">
                                <el-form-item label="超时策略" prop="execTimeoutStrategy">
                                  <!-- <span :title="ruleForm.execTimeoutStrategy" class="truncate-text">{{ ruleForm.execTimeoutStrategy }}</span> -->
                                  <el-tooltip v-if="ruleForm.execTimeoutStrategy && ruleForm.execTimeoutStrategy.length > 25" placement="top" :content="ruleForm.execTimeoutStrategy">
                                    <span :title="ruleForm.execTimeoutStrategy" class="truncate-text">{{ ruleForm.execTimeoutStrategy.length > 25 ? ruleForm.execTimeoutStrategy.substr(0, 25) + '...' : ruleForm.execTimeoutStrategy }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.execTimeoutStrategy" class="truncate-text">{{ ruleForm.execTimeoutStrategy || '--' }}</span>

                                </el-form-item>
                            </el-col>
                            <el-col :span="6">
                                <el-form-item label="执行顺序" prop="execOrderDes">
                                  <!-- <span :title="ruleForm.execOrderDes" class="truncate-text">{{ ruleForm.execOrderDes }}</span> -->
                                  <el-tooltip v-if="ruleForm.execOrderDes && ruleForm.execOrderDes.length > 25" placement="top" :content="ruleForm.execOrderDes">
                                    <span :title="ruleForm.execOrderDes" class="truncate-text">{{ ruleForm.execOrderDes.length > 25 ? ruleForm.execOrderDes.substr(0, 25) + '...' : ruleForm.execOrderDes }}</span>
                                  </el-tooltip>
                                  <span v-else :title="ruleForm.execOrderDes" class="truncate-text">{{ ruleForm.execOrderDes || '--' }}</span>

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


        <!-- <el-table ref="dragTable" :data="columns" row-key="columnId" :max-height="tableHeight">
          <el-table-column label="序号" type="index" min-width="5%"/>
          <el-table-column
            label="字段列名"
            prop="columnName"
            min-width="10%"
            :show-overflow-tooltip="true"
          />
          <el-table-column label="字段描述" min-width="10%">
            <template #default="scope">
              <el-input v-model="scope.row.columnComment"></el-input>
            </template>
          </el-table-column>
          <el-table-column
            label="物理类型"
            prop="columnType"
            min-width="10%"
            :show-overflow-tooltip="true"
          />
          <el-table-column label="Java类型" min-width="11%">
            <template #default="scope">
              <el-select v-model="scope.row.javaType">
                <el-option label="Long" value="Long" />
                <el-option label="String" value="String" />
                <el-option label="Integer" value="Integer" />
                <el-option label="Double" value="Double" />
                <el-option label="BigDecimal" value="BigDecimal" />
                <el-option label="Date" value="Date" />
                <el-option label="Boolean" value="Boolean" />
              </el-select>
            </template>
          </el-table-column>
          <el-table-column label="java属性" min-width="10%">
            <template #default="scope">
              <el-input v-model="scope.row.javaField"></el-input>
            </template>
          </el-table-column>

          <el-table-column label="插入" min-width="5%">
            <template #default="scope">
              <el-checkbox true-label="1" false-label="0" v-model="scope.row.isInsert"></el-checkbox>
            </template>
          </el-table-column>
          <el-table-column label="编辑" min-width="5%">
            <template #default="scope">
              <el-checkbox true-label="1" false-label="0" v-model="scope.row.isEdit"></el-checkbox>
            </template>
          </el-table-column>
          <el-table-column label="列表" min-width="5%">
            <template #default="scope">
              <el-checkbox true-label="1" false-label="0" v-model="scope.row.isList"></el-checkbox>
            </template>
          </el-table-column>
          <el-table-column label="查询" min-width="5%">
            <template #default="scope">
              <el-checkbox true-label="1" false-label="0" v-model="scope.row.isQuery"></el-checkbox>
            </template>
          </el-table-column>
          <el-table-column label="查询方式" min-width="10%">
            <template #default="scope">
              <el-select v-model="scope.row.queryType">
                <el-option label="=" value="EQ" />
                <el-option label="!=" value="NE" />
                <el-option label=">" value="GT" />
                <el-option label=">=" value="GTE" />
                <el-option label="<" value="LT" />
                <el-option label="<=" value="LTE" />
                <el-option label="LIKE" value="LIKE" />
                <el-option label="BETWEEN" value="BETWEEN" />
              </el-select>
            </template>
          </el-table-column>
          <el-table-column label="必填" min-width="5%">
            <template #default="scope">
              <el-checkbox true-label="1" false-label="0" v-model="scope.row.isRequired"></el-checkbox>
            </template>
          </el-table-column>
          <el-table-column label="显示类型" min-width="12%">
            <template #default="scope">
              <el-select v-model="scope.row.htmlType">
                <el-option label="文本框" value="input" />
                <el-option label="文本域" value="textarea" />
                <el-option label="下拉框" value="select" />
                <el-option label="单选框" value="radio" />
                <el-option label="复选框" value="checkbox" />
                <el-option label="日期控件" value="datetime" />
                <el-option label="图片上传" value="imageUpload" />
                <el-option label="文件上传" value="fileUpload" />
                <el-option label="富文本控件" value="editor" />
              </el-select>
            </template>
          </el-table-column>
          <el-table-column label="字典类型" min-width="12%">
            <template #default="scope">
              <el-select v-model="scope.row.dictType" clearable filterable placeholder="请选择">
                <el-option
                  v-for="dict in dictOptions"
                  :key="dict.dictType"
                  :label="dict.dictName"
                  :value="dict.dictType">
                  <span style="float: left">{{ dict.dictName }}</span>
                  <span style="float: right; color: #8492a6; font-size: 13px">{{ dict.dictType }}</span>
              </el-option>
              </el-select>
            </template>
          </el-table-column>
        </el-table> -->
      </el-tab-pane>
      <el-tab-pane label="生成信息" name="genInfo">
        <gen-info-form ref="genInfo" :info="info" :tables="tables" />
      </el-tab-pane>
    </el-tabs>
    <el-form label-width="100px">
      <div style="text-align: center;margin-left:-100px;margin-top:10px;">
        <el-button type="primary" @click="submitForm()">提交</el-button>
        <el-button @click="close()">返回</el-button>
      </div>
    </el-form>
  </el-card>
</template>

<script setup name="GenEdit">
import { getGenTable, updateGenTable } from "@/api/tool/gen";
import { optionselect as getDictOptionselect } from "@/api/system/dict/type";
import basicInfoForm from "./basicInfoForm";
import genInfoForm from "./genInfoForm";

const route = useRoute();
const { proxy } = getCurrentInstance();

const activeName = ref("columnInfo");
const tableHeight = ref(document.documentElement.scrollHeight - 245 + "px");
const tables = ref([]);
const columns = ref([]);
const dictOptions = ref([]);
const info = ref({});

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

const topContentShowFlag = ref(true)
const middleContentShowFlag = ref(true)
const bottomContentShowFlag = ref(true)

// const userStore = useUserStore()
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


function getActionDetailInfoDataTest() {
    const mockData = {
        "code": "00000",
        "message": "success",
        "data": {
            "actionName": "应用申请_用户域名_新建__应用申请_用户域名_新建_29",
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

/** 提交按钮 */
function submitForm() {
  const basicForm = proxy.$refs.basicInfo.$refs.basicInfoForm;
  const genForm = proxy.$refs.genInfo.$refs.genInfoForm;
  Promise.all([basicForm, genForm].map(getFormPromise)).then(res => {
    const validateResult = res.every(item => !!item);
    if (validateResult) {
      const genTable = Object.assign({}, info.value);
      genTable.columns = columns.value;
      genTable.params = {
        treeCode: info.value.treeCode,
        treeName: info.value.treeName,
        treeParentCode: info.value.treeParentCode,
        parentMenuId: info.value.parentMenuId
      };
      updateGenTable(genTable).then(res => {
        proxy.$modal.msgSuccess(res.msg);
        if (res.code === 200) {
          close();
        }
      });
    } else {
      proxy.$modal.msgError("表单校验未通过，请重新检查提交内容");
    }
  });
}

onMounted(() => {
  // const tableId = route.params.tableId;
  // if (tableId) {
  //   getGenTable(tableId).then(res => {
  //     columns.value = res.data.rows;
  //     info.value = res.data.info;
  //     tables.value = res.data.tables;
  //   });
  //   getDictOptionselect().then(response => {
  //     dictOptions.value = response.data;
  //   });
  // }
  getActionDetailInfoDataTest();
});

function getFormPromise(form) {
  return new Promise(resolve => {
    form.validate(res => {
      resolve(res);
    });
  });
}
function close() {
  const obj = { path: "/tool/gen", query: { t: Date.now(), pageNum: route.query.pageNum } };
  proxy.$tab.closeOpenPage(obj);
}

(() => {
  const tableId = route.params && route.params.tableId;
  if (tableId) {
    // 获取表详细信息
    getGenTable(tableId).then(res => {
      columns.value = res.data.rows;
      info.value = res.data.info;
      tables.value = res.data.tables;
    });
    /** 查询字典下拉列表 */
    getDictOptionselect().then(response => {
      dictOptions.value = response.data;
    });
  }
})();
</script>
