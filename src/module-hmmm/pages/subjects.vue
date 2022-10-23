<template>
  <div class="container">
    <el-card class="box-card">
      <div class="clearfix">
        <el-form ref="form" :model="form" label-width="80px">
          <el-col :span="8">
            <el-form-item label="学科名称">
              <el-input
                v-model="form.subjectName"
                placeholder="请输入内容"
              ></el-input>
            </el-form-item>
          </el-col>
          <el-button size="small" class="btn" @click="form.subjectName = ''"
            >清楚</el-button
          >
          <el-button size="small" type="primary" @click="search"
            >搜索</el-button
          >
          <el-button
            size="small"
            style="float: right"
            type="success"
            icon="el-icon-edit"
            @click="open"
            >新增学科</el-button
          >
        </el-form>
      </div>
      <div class="before info">
        <i class="el-icon-info" />
        <span>数据一共{{ counts }}条记录</span>
        <slot name="before" sahb="11"></slot>
      </div>
      <el-table :data="tableData" style="width: 100%">
        <el-table-column type="index" label="序号" width="50" />
        <el-table-column prop="subjectName" label="学科名称" />
        <el-table-column prop="username" label="创建者" />
        <el-table-column prop="addDate" label="创建日期" width="180" />
        <el-table-column prop="isFrontDisplay" label="前台是否显示" />
        <el-table-column prop="twoLevelDirectory" label="二级目录" />
        <el-table-column prop="tags" label="标签" />
        <el-table-column prop="totals" label="题目数量" />
        <el-table-column label="操作" width="240">
          <template slot-scope="{ row }">
            <el-button type="text" @click="xueke(row)">学科分类</el-button>
            <el-button type="text" @click="details(row)">学科标签</el-button>
            <el-button type="text" @click="edit(row)">修改</el-button>
            <el-button type="text" style="color: red" @click="delRow(row)"
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
    <new-subjects
      :form.sync="subject"
      :dialogTableVisible.sync="dialogTableVisible"
      @sx="sx"
    ></new-subjects>
  </div>
</template>

<script>
import NewSubjects from './components/NewSubjects'
import { list, remove } from '@/api/hmmm/subjects'
export default {
  components: {
    NewSubjects
  },
  data () {
    return {
      input: '',
      tableData: [],
      form: {
        page: 1,
        pagesize: 10,
        subjectName: ''
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
  methods: {
    async list () {
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
      console.log(1)
      this.list()
    },
    edit (a) {
      this.open()
      console.log(a)
      this.subject = {
        id: a.id,
        subjectName: a.subjectName,
        isFrontDisplay: a.isFrontDisplay
      }
      console.log(this.subject)
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
        remove(a.id)
        this.$message({
          type: 'success',
          message: '删除成功!'
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    },
    xueke (a) {
      console.log(a)
      this.$router.push({
        path: '/subjects/directorys',
        query: {
          id: a.id
        }
      })
    },
    details (a) {
      console.log(a)
      this.$router.push({
        path: '/subjects/tags',
        query: {
          id: a.id
        }
      })
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
