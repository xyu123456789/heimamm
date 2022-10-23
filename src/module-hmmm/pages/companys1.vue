<template>
  <div class="company">
    <el-card>
      <!-- 搜索区域 -->
      <div>
        <el-form :inline="true" :model="formInline" class="demo-form-inline">
          <el-form-item label="标签名称" label-width="80px">
            <el-col :span="20">
              <el-input
                v-model="formInline.tags"
                placeholder="请输入"
              ></el-input>
            </el-col>
          </el-form-item>
          <el-form-item label="城市" label-width="80px">
            <el-col :span="20">
              <el-select
                v-model="formInline.province"
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
          </el-form-item>
          <el-form-item label="地区" label-width="80px">
            <el-col :span="20">
              <el-select
                v-model="formInline.city"
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
          </el-form-item>
          <el-form-item label="企业简称">
            <el-col :span="20">
              <el-input v-model="formInline.shortName"></el-input>
            </el-col>
          </el-form-item>
          <el-form-item label="状态" label-width="80px">
            <el-col :span="20">
              <el-select v-model="formInline.state" placeholder="请选择">
                <el-option
                  :label="itemState.label"
                  :value="itemState.value"
                  v-for="(itemState, index) in status"
                  :key="index"
                ></el-option>
              </el-select>
            </el-col>
          </el-form-item>
          <el-form-item style="padding-left: 40px">
            <el-row :gutter="10">
              <el-col :span="12">
                <el-button @click="purge">清除</el-button>
              </el-col>
              <el-col :span="12">
                <el-button type="primary" @click="onSubmit">搜索</el-button>
              </el-col>
            </el-row>
          </el-form-item>
          <el-button type="success" @click="addAdmin">
            <el-col :span="20">
              <i class="el-icon-edit"></i>
              <span>新增用户</span>
            </el-col>
          </el-button>
        </el-form>
      </div>

      <el-alert
        :title="`共${total}条记录`"
        type="info"
        show-icon
        :closable="false"
      >
      </el-alert>

      <!-- 企业管理表格展示区 -->
      <el-table :data="tableData" style="width: 100%" v-loading="loading">
        <el-table-column prop="id" label="序号" width="100" align="center">
        </el-table-column>
        <el-table-column
          prop="number"
          label="企业编号"
          width="180"
          align="center"
        >
        </el-table-column>
        <el-table-column
          prop="shortName"
          label="企业简称"
          width="100"
          align="center"
        >
        </el-table-column>
        <el-table-column prop="tags" label="标签" align="center">
        </el-table-column>
        <el-table-column
          prop="creatorID"
          label="创建者"
          width="100"
          align="center"
        >
        </el-table-column>
        <el-table-column prop="addDate" label="创建日期" align="center">
        </el-table-column>
        <el-table-column prop="remarks" label="备注" width="120" align="center">
        </el-table-column>
        <el-table-column
          prop="state"
          label="状态"
          :formatter="manner"
          width="100"
          align="center"
        >
        </el-table-column>
        <el-table-column prop="address" label="操作" align="center">
          <template slot-scope="{ row }">
            <!-- 编辑用户 -->
            <el-button
              type="primary"
              icon="el-icon-edit"
              circle
              plain
              @click="editorAdmin(row)"
            ></el-button>
            <el-button
              type="success"
              :icon="switchButton ? 'el-icon-check' : 'el-icon-close'"
              circle
              plain
            ></el-button>
            <el-button
              type="danger"
              icon="el-icon-delete"
              circle
              plain
            ></el-button>
          </template>
        </el-table-column>
      </el-table>

      <!-- 下一页江南 -->
      <el-row type="flex" justify="end" align="middle" style="height: 60px">
        <el-pagination
          background
          layout="prev, pager, next, sizes, jumper"
          :total="total"
          :page-size.sync="formInline.pagesize"
          :current-page.sync="formInline.page"
          :page-sizes="[10, 20, 30, 50]"
          @size-change="list"
          @current-change="list"
        />
      </el-row>
    </el-card>

    <!-- <increase-admin :dialogVisible.sync="dialogFormVisible" /> -->
    <CompanysAdd
      :dialogFormVisible.sync="dialogFormVisible"
      :newFormBase="startFormBase"
      @newDataes="newDataes"
    />
  </div>
</template>

<script>
import { list } from '../../api/hmmm/companys'
import { provinces, citys } from '../../api/hmmm/citys'
import { status } from '../../api/hmmm/constants'
// import increaseAdmin from '../components/increase-admin.vue'
import CompanysAdd from '../components/companys-add1.vue'

export default {
  components: {
    // increaseAdmin,
    CompanysAdd
  },
  data () {
    return {
      formInline: {
        page: 1,
        pagesize: 10,
        tags: '',
        province: '',
        shortName: '',
        state: '',
        city: ''
      },
      total: 0,
      tableData: [],
      status: status,
      switchButton: true,
      provinces: '',
      citys: '',
      dialogFormVisible: false,
      // startFormBase: {
      //   shortName: '',
      //   company: '',
      //   isFamous: true,
      //   province: '',
      //   city: '',
      //   tags: '',
      //   remarks: ''
      // },
      loading: false,
      rows: ''
    }
  },

  created () {
    this.list()
    this.provinces = provinces()
  },

  methods: {
    // 搜索城市
    onSubmit () {
      this.list()
    },
    async list () {
      this.loading = true
      // 如果值是空字符串，就不加入请求体
      const obj = {}
      for (const key in this.formInline) {
        if (!(this.formInline[key] === '')) {
          obj[key] = this.formInline[key]
        }
      }
      // this.formInline = obj
      const { data } = await list(obj)
      // data.items.forEach((item, index) => {
      //   // this.page.pageIndex当前页 this.page.pageSize 每页显示多少
      //   item.index = (index + 1) + (this.formInline.page - 1) * this.formInline.pagesize
      //   return item
      // })
      this.tableData = data.items
      this.total = data.counts
      this.loading = false
    },
    manner (row, column, cellValue) {
      const res = this.status.find(ele => +ele.value === cellValue)
      return res.label
    },
    // 点击选择城市
    setProvince (node) {
      this.citys = citys(node)
      this.formInline.city = this.citys[0]
    },
    // 清空输入框
    purge () {
      this.formInline = {
        page: 1,
        pagesize: 10,
        tags: '',
        province: '',
        shortName: '',
        state: '',
        city: ''
      }
    },
    // 新增用户弹窗开启
    addAdmin () {
      this.dialogFormVisible = true
    },
    onSubmit1 () {
      console.log('submit!')
    },
    // 新增用户后发送请求重置列表,清空弹出层数据
    newDataes (newFormBase) {
      this.list()
      // console.log(newFormBase)
      // this.newFormBase = {
      //   shortName: '',
      //   company: '',
      //   isFamous: true,
      //   province: '',
      //   city: '',
      //   tags: '',
      //   remarks: ''
      // }
    },
    // 编辑用户
    editorAdmin (row) {
      this.dialogFormVisible = true
      this.startFormBase = row
    }
  }
}
</script>

<style scoped lang='less'>
.company {
  padding: 10px;
}
</style>
