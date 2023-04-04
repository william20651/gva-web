<template>
  <div>
    <div class="gva-form-box">
      <el-form :model="formData" label-position="right" label-width="80px">
        <el-form-item label="name字段use:">
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
        <el-form-item label="出生年月:">
          <el-input v-model.number="formData.born" clearable placeholder="请输入" />
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
        <el-form-item>
          <el-button size="mini" type="primary" @click="save">保存</el-button>
          <el-button size="mini" type="primary" @click="back">返回</el-button>
        </el-form-item>
      </el-form>
    </div>
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
  updateMember,
  findMember
} from '@/api/member'

// 自动获取字典
import { getDictFunc } from '@/utils/format'
import { useRoute, useRouter } from "vue-router"
import { ElMessage } from 'element-plus'
import { ref } from 'vue'
const route = useRoute()
const router = useRouter()
const type = ref('')
const formData = ref({
        name: '',
        realname: '',
        school1: '',
        school2: '',
        school3: '',
        school4: '',
        born: 0,
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
        })

// 初始化方法
const init = async () => {
 // 建议通过url传参获取目标数据ID 调用 find方法进行查询数据操作 从而决定本页面是create还是update 以下为id作为url参数示例
    if (route.query.id) {
      const res = await findMember({ ID: route.query.id })
      if (res.code === 0) {
        formData.value = res.data.remember
        type.value = 'update'
      }
    } else {
      type.value = 'create'
    }
}

init()
// 保存按钮
const save = async() => {
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
      }
}

// 返回按钮
const back = () => {
    router.go(-1)
}

</script>

<style>
</style>
