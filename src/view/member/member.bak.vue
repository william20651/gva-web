<template>
  <div>
    <div class="gva-search-box">
      <el-form :inline="true" :model="searchInfo" class="demo-form-inline">
        <el-form-item>
          <el-button size="small" type="primary" icon="search" @click="onSubmit">查询</el-button>
          <el-button size="small" icon="refresh" @click="onReset">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
    <div class="gva-table-box">
      <div class="gva-btn-list">
        <el-button size="small" type="primary" icon="plus" @click="openDialog">新增</el-button>
        <el-popover v-model:visible="deleteVisible" placement="top" width="160">
          <p>确定要删除吗？</p>
          <div style="text-align: right; margin-top: 8px;">
            <el-button size="small" type="primary" link @click="deleteVisible = false">取消</el-button>
            <el-button size="small" type="primary" @click="onDelete">确定</el-button>
          </div>
          <template #reference>
            <el-button icon="delete" size="small" style="margin-left: 10px;" :disabled="!multipleSelection.length" @click="deleteVisible = true">删除</el-button>
          </template>
        </el-popover>
      </div>
      <el-table
        ref="multipleTable"
        style="width: 100%"
        tooltip-effect="dark"
        :data="tableData"
        row-key="ID"
        @selection-change="handleSelectionChange"
      >
        <el-table-column type="selection" width="55" />
        <el-table-column align="left" label="日期" width="180">
          <template #default="scope">{{ formatDate(scope.row.CreatedAt) }}</template>
        </el-table-column>
        <el-table-column align="left" label="name字段1" prop="name" width="120" />
        <el-table-column align="left" label="真实姓名" prop="realname" width="120" />
        <el-table-column align="left" label="毕业学校1" prop="school1" width="120" />
        <el-table-column align="left" label="school2字段" prop="school2" width="120" />
        <el-table-column align="left" label="school3字段" prop="school3" width="120" />
        <el-table-column align="left" label="school4字段" prop="school4" width="120" />
        <el-table-column align="left" label="出生年" prop="born" width="120" />
        <el-table-column align="left" label="生肖" prop="zodiac" width="120" />
        <el-table-column align="left" label="星座" prop="constellation" width="120" />
        <el-table-column align="left" label="0男1女2保密" prop="gender" width="120" />
        <el-table-column align="left" label="学历,0高中以下，1中专2大专，3本科4硕士5博士" prop="degree" width="120" />
        <el-table-column align="left" label="0未婚1离异2丧偶" prop="marital" width="120" />
        <el-table-column align="left" label="0免费1付费" prop="vip" width="120" />
        <el-table-column align="left" label="职业" prop="job" width="120" />
        <el-table-column align="left" label="0正常1结束（已找到）-1del" prop="stat" width="120" />
        <el-table-column align="left" label="phone字段" prop="phone" width="120" />
        <el-table-column align="left" label="wechat字段" prop="wechat" width="120" />
        <el-table-column align="left" label="头像" prop="photo" width="120" />
        <el-table-column align="left" label="img1字段" prop="img1" width="120" />
        <el-table-column align="left" label="img2字段" prop="img2" width="120" />
        <el-table-column align="left" label="img3字段" prop="img3" width="120" />
        <el-table-column align="left" label="身高厘米" prop="hight" width="120" />
        <el-table-column align="left" label="体重kg" prop="weight" width="120" />
        <el-table-column align="left" label="籍贯" prop="nativePlace" width="120" />
        <el-table-column align="left" label="是否有房" prop="hasHouse" width="120" />
        <el-table-column align="left" label="是否有车" prop="hasCar" width="120" />
        <el-table-column align="left" label="房车补充" prop="ext" width="120" />
        <el-table-column align="left" label="兴趣爱好" prop="interests" width="120" />
        <el-table-column align="left" label="择偶要求" prop="requires" width="120" />
        <el-table-column align="left" label="个人描述" prop="desc" width="120" />
        <el-table-column align="left" label="按钮组">
          <template #default="scope">
            <el-button type="primary" link icon="edit" size="small" class="table-button" @click="updateMemberFunc(scope.row)">变更</el-button>
            <el-button type="primary" link icon="delete" size="small" @click="deleteRow(scope.row)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
      <div class="gva-pagination">
        <el-pagination
          layout="total, sizes, prev, pager, next, jumper"
          :current-page="page"
          :page-size="pageSize"
          :page-sizes="[10, 30, 50, 100]"
          :total="total"
          @current-change="handleCurrentChange"
          @size-change="handleSizeChange"
        />
      </div>
    </div>
    <el-dialog v-model="dialogFormVisible" :before-close="closeDialog" title="弹窗操作">
      <el-form :model="formData" label-position="right" label-width="80px">
        <el-form-item label="name字段2:">
          <el-input v-model="formData.name" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="真实姓名:">
          <el-input v-model="formData.realname" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="毕业学校1:">
          <el-input v-model="formData.school1" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="school2字段:">
          <el-input v-model="formData.school2" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="school3字段:">
          <el-input v-model="formData.school3" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="school4字段:">
          <el-input v-model="formData.school4" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="出生年:">
          <el-input v-model.number="formData.bornYear" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="出生月:">
          <el-input v-model.number="formData.bornMonth" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="生肖:">
          <el-input v-model="formData.zodiac" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="星座:">
          <el-input v-model="formData.constellation" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="0男1女2保密:">
          <el-input v-model.number="formData.gender" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="学历,0高中以下，1中专2大专，3本科4硕士5博士:">
          <el-input v-model.number="formData.degree" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="0未婚1离异2丧偶:">
          <el-input v-model.number="formData.marital" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="0免费1付费:">
          <el-input v-model.number="formData.vip" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="职业:">
          <el-input v-model="formData.job" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="0正常1结束（已找到）-1del:">
          <el-input v-model.number="formData.stat" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="phone字段:">
          <el-input v-model="formData.phone" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="wechat字段:">
          <el-input v-model="formData.wechat" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="头像:">
          <el-input v-model="formData.photo" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="img1字段:">
          <el-input v-model="formData.img1" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="img2字段:">
          <el-input v-model="formData.img2" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="img3字段:">
          <el-input v-model="formData.img3" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="身高厘米:">
          <el-input v-model.number="formData.hight" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="体重kg:">
          <el-input v-model.number="formData.weight" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="籍贯:">
          <el-input v-model="formData.nativePlace" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="是否有房:">
          <el-input v-model.number="formData.hasHouse" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="是否有车:">
          <el-input v-model.number="formData.hasCar" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="房车补充:">
          <el-input v-model="formData.ext" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="兴趣爱好:">
          <el-input v-model="formData.interests" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="择偶要求:">
          <el-input v-model="formData.requires" clearable placeholder="请输入" />
        </el-form-item>
        <el-form-item label="个人描述:">
          <el-input v-model="formData.desc" clearable placeholder="请输入" />
        </el-form-item>
      </el-form>
      <template #footer>
        <div class="dialog-footer">
          <el-button size="small" @click="closeDialog">取 消</el-button>
          <el-button size="small" type="primary" @click="enterDialog">确 定</el-button>
        </div>
      </template>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'Member'
}
</script>

<script setup>
import {
  createMember,
  deleteMember,
  deleteMemberByIds,
  updateMember,
  findMember,
  getMemberList
} from '@/api/member'

// 全量引入格式化工具 请按需保留
import { getDictFunc, formatDate, formatBoolean, filterDict } from '@/utils/format'
import { ElMessage, ElMessageBox } from 'element-plus'
import { ref } from 'vue'

// 自动化生成的字典（可能为空）以及字段
const formData = ref({
  name: '',
  realname: '',
  school1: '',
  school2: '',
  school3: '',
  school4: '',
  bornYear: 0,
  bornMonth: 0,
  zodiac: 0,
  constellation: 0,
  gender: 0,
  degree: 0,
  marital: 0,
  vip: 0,
  job: 0,
  stat: 0,
  phone: '',
  wechat: '',
  photo: '',
  img1: '',
  img2: '',
  img3: '',
  hight: 0,
  weight: 0,
  nativePlace: 0,
  hasHouse: 0,
  hasCar: 0,
  ext: '',
  interests: '',
  requires: '',
  desc: '',
})

// =========== 表格控制部分 ===========
const page = ref(1)
const total = ref(0)
const pageSize = ref(10)
const tableData = ref([])
const searchInfo = ref({})

// 重置
const onReset = () => {
  searchInfo.value = {}
}

// 搜索
const onSubmit = () => {
  page.value = 1
  pageSize.value = 10
  getTableData()
}

// 分页
const handleSizeChange = (val) => {
  pageSize.value = val
  getTableData()
}

// 修改页面容量
const handleCurrentChange = (val) => {
  page.value = val
  getTableData()
}

// 查询
const getTableData = async() => {
  const table = await getMemberList({ page: page.value, pageSize: pageSize.value, ...searchInfo.value })
  if (table.code === 0) {
    tableData.value = table.data.list
    total.value = table.data.total
    page.value = table.data.page
    pageSize.value = table.data.pageSize
  }
}

getTableData()

// ============== 表格控制部分结束 ===============

// 获取需要的字典 可能为空 按需保留
const setOptions = async() => {
}

// 获取需要的字典 可能为空 按需保留
setOptions()

// 多选数据
const multipleSelection = ref([])
// 多选
const handleSelectionChange = (val) => {
  multipleSelection.value = val
}

// 删除行
const deleteRow = (row) => {
  ElMessageBox.confirm('确定要删除吗?', '提示', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',
    type: 'warning'
  }).then(() => {
    deleteMemberFunc(row)
  })
}

// 批量删除控制标记
const deleteVisible = ref(false)

// 多选删除
const onDelete = async() => {
  const ids = []
  if (multipleSelection.value.length === 0) {
    ElMessage({
      type: 'warning',
      message: '请选择要删除的数据'
    })
    return
  }
  multipleSelection.value &&
          multipleSelection.value.map(item => {
            ids.push(item.ID)
          })
  const res = await deleteMemberByIds({ ids })
  if (res.code === 0) {
    ElMessage({
      type: 'success',
      message: '删除成功'
    })
    if (tableData.value.length === ids.length && page.value > 1) {
      page.value--
    }
    deleteVisible.value = false
    getTableData()
  }
}

// 行为控制标记（弹窗内部需要增还是改）
const type = ref('')

// 更新行
const updateMemberFunc = async(row) => {
  const res = await findMember({ ID: row.ID })
  type.value = 'update'
  if (res.code === 0) {
    formData.value = res.data.remember
    dialogFormVisible.value = true
  }
}

// 删除行
const deleteMemberFunc = async(row) => {
  const res = await deleteMember({ ID: row.ID })
  if (res.code === 0) {
    ElMessage({
      type: 'success',
      message: '删除成功'
    })
    if (tableData.value.length === 1 && page.value > 1) {
      page.value--
    }
    getTableData()
  }
}

// 弹窗控制标记
const dialogFormVisible = ref(false)

// 打开弹窗
const openDialog = () => {
  type.value = 'create'
  dialogFormVisible.value = true
}

// 关闭弹窗
const closeDialog = () => {
  dialogFormVisible.value = false
  formData.value = {
    name: '',
    realname: '',
    school1: '',
    school2: '',
    school3: '',
    school4: '',
    bornYear: 0,
    bornMonth: 0,
    zodiac: '',
    constellation: '',
    gender: 0,
    degree: 0,
    marital: 0,
    vip: 0,
    job: '',
    stat: 0,
    phone: '',
    wechat: '',
    photo: '',
    img1: '',
    img2: '',
    img3: '',
    hight: 0,
    weight: 0,
    nativePlace: '',
    hasHouse: 0,
    hasCar: 0,
    ext: '',
    interests: '',
    requires: '',
    desc: '',
  }
}
// 弹窗确定
const enterDialog = async() => {
  let res
  switch (type.value) {
    case 'create':
      res = await createMember(formData.value)
      break
    case 'update':
      res = await updateMember(formData.value)
      break
    default:
      res = await createMember(formData.value)
      break
  }
  if (res.code === 0) {
    ElMessage({
      type: 'success',
      message: '创建/更改成功'
    })
    closeDialog()
    getTableData()
  }
}
</script>

  <style>
  </style>

