<template>
  <div class="add-form">
    <el-dialog
      :visible="dialogFormVisible"
      @close="onClose"
      title="titleInfo.text + titleInfo.pageTitle"
    >
      <el-form
        :rules="ruleInline"
        ref="dataForm"
        :model="formBase"
        label-position="left"
        label-width="150px"
        style="width: 80%; margin-left: 10px"
      >
        <el-form-item label="企业名称" prop="shortName">
          <el-input v-model="formBase.shortName"></el-input>
          <!-- <el-checkbox v-model="formBase.isFamous">是否为名企</el-checkbox> -->
        </el-form-item>
        <el-form-item label="所属公司" prop="company">
          <el-input v-model="formBase.company"></el-input>
          <p>https://www.tianyancha.com （在此可查询所属公司全称及简称）</p>
        </el-form-item>
        <el-form-item label="城市" prop="province">
          <el-row>
            <el-col :span="12">
              <el-form-item label="" prop="province">
                <el-select
                  class="filter-item"
                  style="width: 130px"
                  v-model="formBase.province"
                  @keyup.enter="handleFilter"
                  @change="handleProvince"
                  filterable
                >
                  <el-option
                    v-for="item in citySelect.province"
                    :key="item"
                    :label="item"
                    :value="item"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="" prop="city">
                <el-select
                  class="filter-item"
                  style="width: 130px"
                  v-model="formBase.city"
                  @keyup.enter="handleFilter"
                  filterable
                >
                  <el-option
                    v-for="item in citySelect.cityDate"
                    :key="item"
                    :label="item"
                    :value="item"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form-item>
        <el-form-item label="方向（企业标签）" prop="tags">
          <el-input v-model="formBase.tags"></el-input>
        </el-form-item>
        <el-form-item label="备注" prop="remarks">
          <el-input
            type="textarea"
            :autosize="{ minRows: 2, maxRows: 4 }"
            placeholder="请输入"
            v-model="formBase.remarks"
          ></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormH">{{ $t('table.cancel') }}</el-button>
        <el-button type="primary" @click="createData">{{
          $t('table.confirm')
        }}</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
// import { update, add } from '@/api/base/users'
import { update, add } from '@/api/hmmm/companys'
import { provinces, citys } from '@/api/hmmm/citys.js'
export default {
  name: 'CompanysAdd',
  props: {
    // titleInfo: {
    //   type: Object,
    //   required: true
    // },
    newFormBase: {
      type: Object,
      required: true
    },
    dialogFormVisible: {
      type: Boolean,
      required: true
    }
  },

  data () {
    return {
      // dialogFormVisible: false,
      citySelect: {
        province: [],
        cityDate: []
      },
      formBase: {},
      // this.newFormBase

      // 表单验证
      ruleInline: {
        shortName: [
          { required: true, message: '企业简称不能为空', trigger: 'blur' }
        ],
        province: [
          { required: true, message: '请选择省份', trigger: 'change' }
        ],
        city: [
          { required: true, message: '请选择城市', trigger: 'change' }
        ],
        tags: [{ required: true, message: '请请输标签', trigger: 'blur' }]
      }
    }
  },
  computed: {},
  mounted () {
    console.log(this.formBase)
  },
  watch: {
    // newFormBase () {
    //   this.formBase = { ...this.newFormBase }
    // }
    newFormBase () {
      this.formBase = { ...this.newFormBase }
    }
  },
  methods: {
    // 弹层显示
    // dialogFormV () {
    //   this.dialogFormVisible = true
    // },
    // 弹层隐藏
    dialogFormH () {
      // this.dialogFormVisible = false
      this.$emit('update:dialogFormVisible', false)
    },
    // 关闭弹出层
    onClose () {
      this.formBase = {
        shortName: '',
        company: '',
        isFamous: true,
        province: '',
        city: '',
        tags: '',
        remarks: ''
      }
      this.$refs.dataForm.resetFields()
      this.dialogFormH()
    },
    // 获取省
    getCityData: function () {
      this.citySelect.province = provinces()
    },
    // 选省获取到市
    handleProvince: function (e) {
      this.citySelect.cityDate = citys(e)
      this.formBase.city = this.citySelect.cityDate[0]
    },
    // 表单提交
    createData () {
      this.$refs.dataForm.validate(async (valid) => {
        if (valid) {
          this.dialogFormH()
          const data = {
            ...this.formBase
          }
          if (this.newFormBase.id) {
            // await update(data).then(() => {
            //   this.$emit('newDataes', this.formBase)
            // })

            console.log(data)
            try {
              // 处理数据，删除formBase中接口不需要的属性
              for (const key in data) {
                if (key === 'username' || key === 'addDate' || key === 'state' || key === 'creatorID') {
                  delete data[key]
                }
              }
              data.isFamous = true
              await update(data)
              this.$emit('newDataes', this.formBase)
            } catch (error) {
              this.$message.error(error)
            }
          } else {
            await add(data).then(() => {
              this.$emit('newDataes', this.formBase)
            })
            // try {
            //   await add(this.formBase)
            //   this.$emit('newDataes', this.formBase)
            // } catch (error) {
            //   this.$message.error(error)
            // }
          }
        } else {
          this.$message.error('*号为必填项!')
        }
      })
      // this.$refs.dataForm.resetFields()
    }
  },
  // 挂载结束

  // 创建完毕状态
  created () {
    this.getCityData()
    console.log(this.newFormBase)
  },
  // 组件更新
  updated: function () { }
}
</script>
<style>
.el-form--label-left .el-form-item__label {
  text-align: right;
}
.el-form-item--medium {
  margin-bottom: 22px;
}
.el-dialog__footer {
  text-align: center;
}
</style>
