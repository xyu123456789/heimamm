<template>
  <!-- 弹出层 -->
  <el-dialog
    title="收货地址"
    :visible="dialogVisible"
    center
    @close="resetForm"
  >
    <el-form
      ref="addAdminInfo"
      :model="addAdminInfo"
      label-width="80px"
      :rules="rules"
    >
      <el-form-item label="企业名称" label-width="150px" prop="ename">
        <el-col :span="18">
          <el-input v-model="addAdminInfo.ename"></el-input>
        </el-col>
      </el-form-item>
      <el-form-item label="所属公司" label-width="150px" prop="firm">
        <el-col :span="18">
          <el-input v-model="addAdminInfo.firm"></el-input>
          <span
            >https://www.tianyancha.com （在此可查询所属公司全称及简称）</span
          >
        </el-col>
      </el-form-item>
      <el-form-item label="城市地区" label-width="150px">
        <el-row :gutter="6">
          <el-col :span="9">
            <el-select
              v-model="addAdminInfo.province"
              placeholder="请选择"
              filterable
              @change="setProvince"
            >
              <el-option
                :label="item"
                :value="item"
                v-for="(item, index) in provinces"
                :key="index"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="9">
            <el-select
              v-model="addAdminInfo.city"
              placeholder="请选择"
              filterable
            >
              <el-option
                :label="itemcity"
                :value="itemcity"
                v-for="(itemcity, index) in citys"
                :key="index"
              />
            </el-select>
          </el-col>
        </el-row>
      </el-form-item>
      <el-form-item
        label="方向（企业标签）"
        label-width="150px"
        prop="direction"
      >
        <el-col :span="18">
          <el-input v-model="addAdminInfo.direction"></el-input>
        </el-col>
      </el-form-item>
      <el-form-item label="备注" label-width="150px" prop="remark">
        <el-col :span="18">
          <el-input type="textarea" v-model="addAdminInfo.remark"></el-input>
        </el-col>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="resetForm">取 消</el-button>
      <el-button type="primary" @click="dialogFormVisible = false"
        >确 定</el-button
      >
    </span>
  </el-dialog>
</template>

<script>
import { provinces, citys } from '../../api/hmmm/citys'
export default {
  props: {
    dialogVisible: {
      type: Boolean,
      required: false
    }
  },
  data () {
    return {
      provinces: '',
      citys: '',
      addAdminInfo: {
        ename: '',
        firm: '',
        province: '',
        city: '',
        direction: '',
        remark: ''
      },
      rules: {
        ename: [{ required: true, message: '企业名称不能为空', trigger: 'blur' }],
        firm: [{ required: true, message: '所属公司不能为空', trigger: 'blur' }],
        direction: [{ required: true, message: '请输入标签', trigger: 'blur' }],
        remark: [{ required: true, message: '备注不能为空', trigger: 'blur' }]
      }
    }
  },
  created () {
    this.provinces = provinces()
  },
  methods: {
    // 点击选择城市
    setProvince (node) {
      this.citys = citys(node)
      this.addAdminInfo.city = this.citys[0]
    },
    // 重置弹出层
    resetForm () {
      this.$refs.addAdminInfo.resetFields()
      this.addAdminInfo.city = ''
      this.addAdminInfo.province = ''
      this.$emit('update:dialogVisible', false)
    }
  }
}
</script>

<style>
</style>
