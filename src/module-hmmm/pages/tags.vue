<template>
  <div class="container">
    <el-card class="box-card">
      <div class="clearfix">
        <el-form ref="form" :model="form" label-width="80px">
          <el-col :span="4">
            <el-form-item label="标签名称">
              <el-input
                v-model="form.tagName"
                placeholder="请输入内容"
                width="80px"
              ></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="4">
            <el-form-item label="状态">
              <el-select v-model="form.state" placeholder="请选择">
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                >
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-button
            size="small"
            style="float: right"
            type="success"
            icon="el-icon-edit"
            @click="open"
            >新增学科</el-button
          >
        </el-form>
        <div>
          <el-button size="small" class="btn" @click="onclose">清除</el-button>
          <el-button size="small" type="primary" @click="search"
            >搜索</el-button
          >
        </div>
      </div>
      <div class="before info">
        <i class="el-icon-info" />
        <span>数据一共{{ counts }}条记录</span>
        <slot name="before" sahb="11"></slot>
      </div>
      <el-table :data="tableData" style="width: 100%">
        <el-table-column type="index" label="序号" width="50" />
        <el-table-column prop="subjectName" label="所属学科" />
        <el-table-column prop="tagName" label="标签名称" />
        <el-table-column prop="username" label="创建者" />
        <el-table-column prop="addDate" label="创建日期" width="180" />
        <el-table-column prop="state" label="状态" :formatter="manner" />
        <el-table-column label="操作" width="240">
          <template slot-scope="{ row }">
            <el-button type="text" v-if="row.state === 1" @click="details(row)"
              >禁用</el-button
            >
            <el-button type="text" v-if="row.state === 0" @click="details(row)"
              >启用</el-button
            >
            <el-button
              type="text"
              :disabled="row.state === 1"
              @click="edit(row)"
              >修改</el-button
            >
            <el-button
              type="text"
              :disabled="row.state === 1"
              @click="delRow(row)"
              >删除</el-button
            >
          </template>
        </el-table-column>
      </el-table>
      <div class="block">
        <el-pagination
          background
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage4"
          :page-sizes="[5, 10, 20, 50]"
          :page-size="10"
          layout="total, sizes, prev, pager, next, jumper"
          :total="counts"
        >
        </el-pagination>
      </div>
    </el-card>
    <new-tags
      :form.sync="subject"
      :dialogTableVisible.sync="dialogTableVisible"
      @sx="sx"
    ></new-tags>
  </div>
</template>

<script>
import NewTags from './components/NewTags.vue'
import { list, remove } from '@/api/hmmm/tags'
import { status } from '@/api/hmmm/constants'

export default {
  components: {
    NewTags
  },
  data () {
    return {
      value: '',
      options: [{
        value: '1',
        label: '启用'
      }, {
        value: '0',
        label: '禁用'
      }],
      input: '',
      tableData: [],
      form: {
        page: 1,
        pagesize: 10,
        state: null
      },
      counts: 0,
      currentPage4: 1,
      dialogTableVisible: false,
      subject: {}
    }
  },
  created () {
    this.list()
  },
  mounted () {
  },
  methods: {
    async list () {
      if (this.$route.query.id) {
        this.form.subjectID = this.$route.query.id
      }
      const { data } = await list(this.form)
      console.log(data)
      this.tableData = data.items
      this.counts = data.counts
    },
    handleSizeChange (val) {
      console.log(`每页 ${val} 条`)
      this.form.pagesize = val
      this.list()
    },
    handleCurrentChange (val) {
      console.log(`当前页: ${val}`)
      this.form.page = val
      this.list()
    },
    search () {
      if (this.value !== '') {
        this.form.state = this.value ? 1 : 0
      }
      console.log(this.value)
      this.list()
    },
    onclose () {
      this.form.state = null
      this.form.directoryName = ''
    },
    edit (a) {
      this.open()
      console.log(a)
      this.subject = {
        id: a.id,
        subjectID: a.subjectID,
        directoryName: a.directoryName
      }
      console.log(this.subject)
    },
    details (row) {
      row.state = row.state === 1 ? 0 : 1
    },
    open () {
      this.dialogTableVisible = true
    },
    sx () {
      this.list()
    },
    async delRow (a) {
      this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        remove(a)
        this.$message({
          type: 'success',
          message: '删除成功!'
        })
        this.list()
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    },
    manner (row, column, cellValue) {
      const res = status.find(ele => +ele.value === cellValue)
      return res.label
    }
  }
}
</script>

<style scoped lang='less'>
.before {
  width: 100%;
  line-height: 34px;
  display: inline-block;
  border-radius: 3px;
  i {
    margin-right: 5px;
  }

  &.info {
    padding: 0px 10px;
    border: 1px solid #f4f4f5;
    background: #f4f4f5;
    i {
      color: #909399;
    }
  }
}
.btn {
  margin: 0px 10px;
}
.block {
  text-align: right;
  margin-top: 20px;
}
</style>
