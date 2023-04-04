<!-- eslint-disable vue/no-parsing-error -->
<template>
  <div>
    <div class="gva-search-box">
      <el-form :inline="true" :model="searchInfo" class="demo-form-inline">
        <el-form-item>
          <el-form-item label="手机号:">
            <el-input v-model="searchInfo.phone" clearable placeholder="请输入手机号" />
          </el-form-item>
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

        <el-table-column align="left" label="用户名" prop="name" width="120" />
        <el-table-column align="left" label="真实姓名" prop="realname" width="120" />
        <el-table-column align="left" label="第一毕业学校" prop="school1" width="120" />
        <el-table-column align="left" label="第二毕业学校" prop="school2" width="120" />
        <el-table-column align="left" label="第三毕业学校" prop="school3" width="120" />
        <el-table-column align="left" label="第四毕业学校" prop="school4" width="120" />
        <el-table-column align="left" label="出生日期" prop="born" width="120">
          <template #default="scope">{{ formatDateOnlyDate(scope.row.born) }}</template>
        </el-table-column>
        <el-table-column align="left" label="生肖" prop="zodiac" width="120">
          <template #default="scope">{{ formatZodiac(scope.row.zodiac) }}</template>
        </el-table-column>
        <el-table-column align="left" label="星座" prop="constellation" width="120">
          <template #default="scope">{{ formatConstellation(scope.row.constellation) }}</template>
        </el-table-column>
        <el-table-column align="left" label="性别" prop="gender" width="120">
          <template #default="scope">{{ formatGender(scope.row.gender) }}</template>
        </el-table-column>
        <el-table-column align="left" label="学历" prop="degree" width="120">
          <template #default="scope">{{ formatDegree(scope.row.degree) }}</template>
        </el-table-column>
        <el-table-column align="left" label="婚姻状况" prop="marital" width="120">
          <template #default="scope">{{ formatMarital(scope.row.marital) }}</template>
        </el-table-column>
        <el-table-column align="left" label="是否会员" prop="vip" width="120">
          <template #default="scope">{{ formatVip(scope.row.vip) }}</template>
        </el-table-column>
        <el-table-column align="left" label="职业" prop="job" width="120">
          <template #default="scope">{{ formatJob(scope.row.job) }}</template>
        </el-table-column>
        <el-table-column align="left" label="征婚状态" prop="stat" width="120">
          <template #default="scope">{{ formatStat(scope.row.stat) }}</template>
        </el-table-column>
        <el-table-column align="left" label="phone" prop="phone" width="120" />
        <el-table-column align="left" label="wechat" prop="wechat" width="120" />
        <el-table-column align="left" label="头像" prop="photo" width="120" />
        <el-table-column align="left" label="img1" prop="img1" width="120" />
        <el-table-column align="left" label="img2" prop="img2" width="120" />
        <el-table-column align="left" label="img3" prop="img3" width="120" />
        <el-table-column align="left" label="身高(cm)" prop="hight" width="120" />
        <el-table-column align="left" label="体重(kg)" prop="weight" width="120" />
        <el-table-column align="left" label="籍贯" prop="nativePlace" width="120" />
        <el-table-column align="left" label="是否有房" prop="hasHouse" width="120">
          <template #default="scope">{{ formatHasHouse(scope.row.hasHouse) }}</template>
        </el-table-column>
        <el-table-column align="left" label="是否有车" prop="hasCar" width="120">
          <template #default="scope">{{ formatHasCar(scope.row.hasCar) }}</template>
        </el-table-column>
        <el-table-column align="left" label="房车补充" prop="ext" width="120" />
        <el-table-column align="left" label="兴趣爱好" prop="interests" width="120" />
        <el-table-column align="left" label="择偶要求" prop="requires" width="120" />
        <el-table-column align="left" label="个人描述" prop="desc" width="120" />
        <el-table-column align="left" label="添加日期" width="180">
          <template #default="scope">{{ formatDate(scope.row.CreatedAt) }}</template>
        </el-table-column>
        <el-table-column align="left" label="按钮组">
          <template #default="scope">
            <el-button type="primary" link icon="edit" size="small" class="table-button" @click="updateMemberFunc(scope.row)">变更</el-button>
            <el-button type="primary" link icon="delete" size="small" @click="deleteRow(scope.row)">删除</el-button>
          </template>
        </el-table-column></el-table>
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
        <el-form-item label="用户名" prop="name">
          <el-input
            v-model="formData.name"
            placeholder="请输入用户名"
            :maxlength="11"
            show-word-limit
            clearable
            prefix-icon="el-icon-mobile"
            :style="{width: '100%'}"
          />
        </el-form-item>
        <el-form-item label="真实姓名" prop="realname">
          <el-input v-model="formData.realname" placeholder="请输入真实姓名" clearable :style="{width: '100%'}" />
        </el-form-item>
        <el-form-item label="性别" prop="gender">
          <el-radio-group v-model="formData.gender" size="default">
            <el-radio
              v-for="(item, index) in genderOptions"
              :key="index"
              :label="item.value"
              :disabled="item.disabled"
            >{{ item.label }}</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="手机号" prop="phone">
          <el-input v-model="formData.phone" placeholder="请输入手机号" clearable :style="{width: '100%'}" />
        </el-form-item>
        <el-form-item label="微信号" prop="wechat">
          <el-input v-model="formData.wechat" placeholder="请输入微信号" clearable :style="{width: '100%'}" />
        </el-form-item>
        <el-form-item label="出生日期" prop="born">
          <el-date-picker
            v-model="formData.born"
            :style="{width: '100%'}"
            placeholder="请选择出生日期"
            clearable
          />
        </el-form-item>
        <el-form-item label="籍贯" prop="nativePlace">
          <el-cascader
            v-model="formData.nativePlace"
            :options="nativePlaceOptions"
            :props="nativePlaceProps"
            :style="{width: '100%'}"
            placeholder="请选择籍贯"
            clearable
          />
        </el-form-item>
        <el-form-item label="身高(厘米)" prop="hight">
          <el-input v-model="formData.hight" placeholder="请输入身高" clearable :style="{width: '100%'}" />
        </el-form-item>
        <el-form-item label="体重(千克)" prop="weight">
          <el-input v-model="formData.weight" placeholder="请输入体重" clearable :style="{width: '100%'}" />
        </el-form-item>
        <el-form-item label="生肖" prop="zodiac">
          <el-select v-model="formData.zodiac" placeholder="请选择生肖" clearable :style="{width: '100%'}">
            <el-option
              v-for="(item, index) in zodiacOptions"
              :key="index"
              :label="item.label"
              :value="item.value"
              :disabled="item.disabled"
            />
          </el-select>
        </el-form-item>
        <el-form-item label="星座" prop="constellation">
          <el-select v-model="formData.constellation" placeholder="请选择星座" clearable :style="{width: '100%'}">
            <el-option
              v-for="(item, index) in constellationOptions"
              :key="index"
              :label="item.label"
              :value="item.value"
              :disabled="item.disabled"
            />
          </el-select>
        </el-form-item>
        <el-form-item label="婚育" prop="marital">
          <el-select v-model="formData.marital" placeholder="请选择婚育" clearable :style="{width: '100%'}">
            <el-option
              v-for="(item, index) in maritalOptions"
              :key="index"
              :label="item.label"
              :value="item.value"
              :disabled="item.disabled"
            />
          </el-select>
        </el-form-item>
        <el-form-item label="职业" prop="job">
          <el-select v-model="formData.job" placeholder="请选择职业" clearable :style="{width: '100%'}">
            <el-option
              v-for="(item, index) in jobOptions"
              :key="index"
              :label="item.label"
              :value="item.value"
              :disabled="item.disabled"
            />
          </el-select>
        </el-form-item>
        <el-form-item label="状态" prop="stat">
          <el-radio-group v-model="formData.stat" size="default">
            <el-radio
              v-for="(item, index) in statOptions"
              :key="index"
              :label="item.value"
              :disabled="item.disabled"
            >{{ item.label }}</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="是否有车" prop="hasCar">
          <el-radio-group v-model="formData.hasCar" size="default">
            <el-radio
              v-for="(item, index) in hasCarOptions"
              :key="index"
              :label="item.value"
              :disabled="item.disabled"
            >{{ item.label }}</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="是否有房" prop="hasHouse">
          <el-radio-group v-model="formData.hasHouse" size="default">
            <el-radio
              v-for="(item, index) in hasHouseOptions"
              :key="index"
              :label="item.value"
              :disabled="item.disabled"
            >{{ item.label }}</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="房车补充" prop="ext">
          <el-input v-model="formData.ext" placeholder="请输入房车补充" clearable :style="{width: '100%'}" />
        </el-form-item>
        <el-form-item label="学历" prop="degree">
          <el-select v-model="formData.degree" placeholder="请选择学历" clearable :style="{width: '100%'}">
            <el-option
              v-for="(item, index) in degreeOptions"
              :key="index"
              :label="item.label"
              :value="item.value"
              :disabled="item.disabled"
            />
          </el-select>
        </el-form-item>
        <el-form-item label="毕业学校1" prop="school1">
          <el-input v-model="formData.school1" placeholder="请输入毕业学校1" clearable :style="{width: '100%'}" />
        </el-form-item>
        <el-form-item label="毕业学校2" prop="school2">
          <el-input v-model="formData.school2" placeholder="请输入毕业学校2" clearable :style="{width: '100%'}" />
        </el-form-item>
        <el-form-item label="毕业学校3" prop="school3">
          <el-input v-model="formData.school3" placeholder="请输入毕业学校3" clearable :style="{width: '100%'}" />
        </el-form-item>
        <el-form-item label="毕业学校4" prop="school4">
          <el-input v-model="formData.school4" placeholder="请输入毕业学校4" clearable :style="{width: '100%'}" />
        </el-form-item>
        <el-form-item label="是否会员" prop="vip">
          <el-radio-group v-model="formData.vip" size="default">
            <el-radio
              v-for="(item, index) in vipOptions"
              :key="index"
              :label="item.value"
              :disabled="item.disabled"
            >{{ item.label }}</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="头像" prop="photo" required>
          <el-upload
            ref="photo"
            :file-list="photofileList"
            :action="photoAction"
            :before-upload="photoBeforeUpload"
          >
            <el-button size="small" type="primary" icon="el-icon-upload">点击上传</el-button>
          </el-upload>
        </el-form-item>
        <el-form-item label="全身照1" prop="img1" required>
          <el-upload ref="img1" :file-list="img1fileList" :action="img1Action" :before-upload="img1BeforeUpload">
            <el-button size="small" type="primary" icon="el-icon-upload">点击上传</el-button>
          </el-upload>
        </el-form-item>
        <el-form-item label="全身照2" prop="img2" required>
          <el-upload ref="img2" :file-list="img2fileList" :action="img2Action" :before-upload="img2BeforeUpload">
            <el-button size="small" type="primary" icon="el-icon-upload">点击上传</el-button>
          </el-upload>
        </el-form-item>
        <el-form-item label="全身照3" prop="img3" required>
          <el-upload ref="img3" :file-list="img3fileList" :action="img3Action" :before-upload="img3BeforeUpload">
            <el-button size="small" type="primary" icon="el-icon-upload">点击上传</el-button>
          </el-upload>
        </el-form-item>
        <el-form-item label="兴趣爱好" prop="interests">
          <el-input
            v-model="formData.interests"
            type="textarea"
            placeholder="请输入兴趣爱好"
            :autosize="{minRows: 4, maxRows: 4}"
            :style="{width: '100%'}"
          />
        </el-form-item>
        <el-form-item label="择偶要求" prop="requires">
          <el-input
            v-model="formData.requires"
            type="textarea"
            placeholder="请输入择偶要求"
            :autosize="{minRows: 4, maxRows: 4}"
            :style="{width: '100%'}"
          />
        </el-form-item>
        <el-form-item label="补充条件" prop="desc">
          <el-input
            v-model="formData.desc"
            type="textarea"
            placeholder="请输入补充条件"
            :autosize="{minRows: 4, maxRows: 4}"
            :style="{width: '100%'}"
          />
        </el-form-item>
        <el-form-item size="large">
          <el-button type="primary" @click="submitForm">提交</el-button>
          <el-button @click="resetForm">重置</el-button>
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
  name: 'Member',
  components: {},
  props: [],
  data() {
    return {
      formData: {
        name: '',
        realname: '',
        gender: 1,
        phone: '',
        wechat: '',
        born: '',
        nativePlace: [],
        hight: 0,
        weight: 0,
        zodiac: 0,
        constellation: 0,
        marital: 0,
        job: 0,
        stat: 1,
        hasCar: 2,
        hasHouse: 2,
        ext: '',
        degree: '',
        school1: '',
        school2: '',
        school3: '',
        school4: '',
        vip: 1,
        photo: null,
        img1: null,
        img2: null,
        img3: null,
        interests: '',
        requires: '',
        desc: '',
      },
      rules: {
        name: [{
          required: true,
          message: '请输入名用户用户名用户名用户名',
          trigger: 'blur'
        }, {
          pattern: /^1(3|4|5|7|8|9)\d{9}$/,
          message: '手机号格式错误',
          trigger: 'blur'
        }],
        realname: [{
          required: true,
          message: '请输入真实姓名',
          trigger: 'blur'
        }],
        gender: [{
          required: true,
          message: '性别不能为空',
          trigger: 'change'
        }],
        phone: [{
          required: true,
          message: '请输入手机号',
          trigger: 'blur'
        }],
        wechat: [{
          required: true,
          message: '请输入微信号',
          trigger: 'blur'
        }],
        born: [{
          required: true,
          message: '请选择出生日期',
          trigger: 'change'
        }],
        nativePlace: [{
          required: true,
          type: 'array',
          message: '请至少选择一个籍贯',
          trigger: 'change'
        }],
        hight: [{
          required: true,
          message: '请输入身高',
          trigger: 'blur'
        }],
        weight: [{
          required: true,
          message: '请输入体重',
          trigger: 'blur'
        }],
        zodiac: [{
          required: true,
          message: '请选择生肖',
          trigger: 'change'
        }],
        constellation: [{
          required: true,
          message: '请选择星座',
          trigger: 'change'
        }],
        marital: [{
          required: true,
          message: '请选择婚育',
          trigger: 'change'
        }],
        job: [{
          required: true,
          message: '请选择职业',
          trigger: 'change'
        }],
        stat: [{
          required: true,
          message: '状态不能为空',
          trigger: 'change'
        }],
        hasCar: [{
          required: true,
          message: '是否有车不能为空',
          trigger: 'change'
        }],
        hasHouse: [{
          required: true,
          message: '是否有房不能为空',
          trigger: 'change'
        }],
        ext: [{
          required: true,
          message: '请输入房车补充',
          trigger: 'blur'
        }],
        degree: [{
          required: true,
          message: '请选择学历',
          trigger: 'change'
        }],
        school1: [{
          required: true,
          message: '请输入毕业学校1',
          trigger: 'blur'
        }],
        school2: [{
          required: true,
          message: '请输入毕业学校2',
          trigger: 'blur'
        }],
        school3: [{
          required: true,
          message: '请输入毕业学校3',
          trigger: 'blur'
        }],
        school4: [{
          required: true,
          message: '请输入毕业学校4',
          trigger: 'blur'
        }],
        vip: [{
          required: true,
          message: '是否会员不能为空',
          trigger: 'change'
        }],
        interests: [{
          required: true,
          message: '请输入兴趣爱好',
          trigger: 'blur'
        }],
        requires: [{
          required: true,
          message: '请输入择偶要求',
          trigger: 'blur'
        }],
        desc: [{
          required: true,
          message: '请输入补充条件',
          trigger: 'blur'
        }],
      },
      photoAction: 'https://jsonplaceholder.typicode.com/posts/',
      photofileList: [],
      img1Action: 'https://jsonplaceholder.typicode.com/posts/',
      img1fileList: [],
      img2Action: 'https://jsonplaceholder.typicode.com/posts/',
      img2fileList: [],
      img3Action: 'https://jsonplaceholder.typicode.com/posts/',
      img3fileList: [],
      genderOptions: [{
        'label': '男',
        'value': 1
      }, {
        'label': '女',
        'value': 2
      }],
      nativePlaceOptions: [],
      zodiacOptions: [{
        'label': '鼠',
        'value': 1
      }, {
        'label': '牛',
        'value': 2
      }, {
        'label': '虎',
        'value': 3
      }, {
        'label': '兔',
        'value': 4
      }, {
        'label': '龙',
        'value': 5
      }, {
        'label': '蛇',
        'value': 6
      }, {
        'label': '马',
        'value': 7
      }, {
        'label': '羊',
        'value': 8
      }, {
        'label': '猴',
        'value': 9
      }, {
        'label': '鸡',
        'value': 10
      }, {
        'label': '狗',
        'value': 11
      }, {
        'label': '猪',
        'value': 12
      }],
      constellationOptions: [{
        'label': '白羊',
        'value': 1
      }, {
        'label': '金牛',
        'value': 2
      }, {
        'label': '双子',
        'value': 3
      }, {
        'label': '巨蟹',
        'value': 4
      }, {
        'label': '狮子',
        'value': 5
      }, {
        'label': '处女',
        'value': 6
      }, {
        'label': '天秤',
        'value': 7
      }, {
        'label': '天蝎',
        'value': 8
      }, {
        'label': '射手',
        'value': 9
      }, {
        'label': '摩羯',
        'value': 10
      }, {
        'label': '水瓶',
        'value': 11
      }, {
        'label': '双鱼',
        'value': 12
      }],
      maritalOptions: [{
        'label': '未婚',
        'value': 1
      }, {
        'label': '离异',
        'value': 2
      }, {
        'label': '丧偶',
        'value': 3
      }],
      jobOptions: [{
        'label': '科研',
        'value': 1
      }, {
        'label': '工程师',
        'value': 2
      }, {
        'label': '销售',
        'value': 3
      }, {
        'label': '经商',
        'value': 4
      }],
      statOptions: [{
        'label': '征婚',
        'value': 1
      }, {
        'label': '不征',
        'value': 2
      }],
      hasCarOptions: [{
        'label': '有',
        'value': 1
      }, {
        'label': '无',
        'value': 2
      }],
      hasHouseOptions: [{
        'label': '有',
        'value': 1
      }, {
        'label': '无',
        'value': 2
      }],
      degreeOptions: [{
        'label': '高中以下',
        'value': 1
      }, {
        'label': '高中/中专',
        'value': 2
      }, {
        'label': '大专',
        'value': 3
      }, {
        'label': '本科',
        'value': 4
      }, {
        'label': '硕士',
        'value': 5
      }, {
        'label': '博士',
        'value': 6
      }],
      vipOptions: [{
        'label': '免费',
        'value': 1
      }, {
        'label': '付费',
        'value': 2
      }],
      nativePlaceProps: {
        'multiple': false,
        'label': 'label',
        'value': 'value',
        'children': 'children'
      },
    }
  },
  computed: {},
  watch: {},
  created() {
    this.getNativePlaceOptions()
  },
  mounted() {},
  methods: {
    submitForm() {
      this.$refs['elForm'].validate(valid => {
        if (!valid) return
        // TODO 提交表单
      })
    },
    resetForm() {
      this.$refs['elForm'].resetFields()
    },
    getNativePlaceOptions() {
      // 注意：this.$axios是通过Vue.prototype.$axios = axios挂载产生的
      // this.$axios({
      //   method: 'get',
      //   url: 'http://localhost:8080/api/area/getAreaList',
      // }).then(resp => {
      //   var {
      //     data
      //   } = resp
      //   this.nativePlaceOptions = data.list
      // })
      const areaList = getAreaListTop
      if (areaList.code === 0) {
        this.nativePlaceOptions = areaList.data
      }
      // const area = getAreaList
      // console.log('sss', area)
      // console.log('tttttt', areaList)
      // this.nativePlaceOptions = getAreaListTop
    },
    photoBeforeUpload(file) {
      const isRightSize = file.size / 1024 / 1024 < 2
      if (!isRightSize) {
        this.$message.error('文件大小超过 2MB')
      }
      return isRightSize
    },
    img1BeforeUpload(file) {
      const isRightSize = file.size / 1024 / 1024 < 2
      if (!isRightSize) {
        this.$message.error('文件大小超过 2MB')
      }
      return isRightSize
    },
    img2BeforeUpload(file) {
      const isRightSize = file.size / 1024 / 1024 < 2
      if (!isRightSize) {
        this.$message.error('文件大小超过 2MB')
      }
      return isRightSize
    },
    img3BeforeUpload(file) {
      const isRightSize = file.size / 1024 / 1024 < 2
      if (!isRightSize) {
        this.$message.error('文件大小超过 2MB')
      }
      return isRightSize
    },
    formatZodiac(val) {
      for (const value of this.zodiacOptions) {
        if (val !== null && value.value === val) {
          return value.label
        }
      }
    },
    formatConstellation(val) {
      for (const value of this.constellationOptions) {
        if (val !== null && value.value === val) {
          return value.label
        }
      }
    },
    formatGender(val) {
      for (const value of this.genderOptions) {
        if (val !== null && value.value === val) {
          return value.label
        }
      }
    },
    formatDegree(val) {
      for (const value of this.degreeOptions) {
        if (val !== null && value.value === val) {
          return value.label
        }
      }
    },
    formatMarital(val) {
      for (const value of this.maritalOptions) {
        if (val !== null && value.value === val) {
          return value.label
        }
      }
    },
    formatVip(val) {
      for (const value of this.vipOptions) {
        if (val !== null && value.value === val) {
          return value.label
        }
      }
    },
    formatJob(val) {
      for (const value of this.jobOptions) {
        if (val !== null && value.value === val) {
          return value.label
        }
      }
    },
    formatStat(val) {
      for (const value of this.statOptions) {
        if (val !== null && value.value === val) {
          return value.label
        }
      }
    },
    formatHasHouse(val) {
      for (const value of this.hasHouseOptions) {
        if (val !== null && value.value === val) {
          return value.label
        }
      }
    },
    formatHasCar(val) {
      for (const value of this.hasCarOptions) {
        if (val !== null && value.value === val) {
          return value.label
        }
      }
    },
  }
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
import { getDictFunc, formatDate, formatDateOnlyDate, formatBoolean, filterDict } from '@/utils/format'
import { getAreaListTop, getAreaList } from '@/api/area'
import { ElMessage, ElMessageBox } from 'element-plus'
import { ref } from 'vue'
import { forIn } from 'lodash'

// 自动化生成的字典（可能为空）以及字段
const formData = ref({
  name: '',
  realname: '',
  gender: 1,
  phone: '',
  wechat: '',
  born: '',
  nativePlace: [],
  hight: 0,
  weight: 0,
  zodiac: 0,
  constellation: 0,
  marital: 0,
  job: 0,
  stat: 1,
  hasCar: 2,
  hasHouse: 2,
  ext: '',
  degree: '',
  school1: '',
  school2: '',
  school3: '',
  school4: '',
  vip: 1,
  photo: null,
  img1: null,
  img2: null,
  img3: null,
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
    gender: 1,
    phone: '',
    wechat: '',
    born: '',
    nativePlace: [],
    hight: 0,
    weight: 0,
    zodiac: 0,
    constellation: 0,
    marital: 0,
    job: 0,
    stat: 1,
    hasCar: 2,
    hasHouse: 2,
    ext: '',
    degree: '',
    school1: '',
    school2: '',
    school3: '',
    school4: '',
    vip: 1,
    photo: null,
    img1: null,
    img2: null,
    img3: null,
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
.el-upload__tip {
  line-height: 1.2;
}
</style>
