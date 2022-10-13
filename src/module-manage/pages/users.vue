<template>
  <div class="app-container">
    <el-card class="box-card">
      <div class="card-info">
        <el-input placeholder="请输入内容" type="text"></el-input>
        <el-button>清空</el-button>
        <el-button type="primary"> <i class="el-icon-edit"></i>搜索</el-button>
        <el-button type="success">新增用户</el-button>
      </div>
      <div class="info">
        <i class="el-icon-info"></i>
        <span>共1条记录</span>
      </div>
      <template>
        <el-table :data="tableData" style="width: 100%">
          <el-table-column prop="date" label="序号" width="180" align="center">
          </el-table-column>
          <el-table-column prop="name" label="邮箱" width="180" align="center">
          </el-table-column>
          <el-table-column prop="address" label="联系电话" align="center">
          </el-table-column>
          <el-table-column
            prop="user"
            label="用户名"
            width="180"
            align="center"
          >
          </el-table-column>
          <el-table-column
            prop="quanx"
            label="权限组名称"
            width="180"
            align="center"
          >
          </el-table-column>
          <el-table-column prop="jiaos" label="角色" width="180" align="center">
          </el-table-column>
          <el-table-column prop="date" label="操作" width="180" align="center">
          </el-table-column>
        </el-table>
      </template>
      <el-row type="flex" justify="end" align="middle" style="height: 60px">
        <el-pagination background layout="prev, pager, next,size,total" :total="total1">
        </el-pagination>
      </el-row>
    </el-card>
  </div>
</template>

<script>
export default {
  data () {
    return {
      tableData: [
        {
          date: '2',
          name: 'root@admin.com',
          address: '123456789',
          user: '超级管2',
          quanx: '权限管理员组',
          jiaos: 'admin1'
        }
      ],
      total1: 0
    }
  },

  created () {},

  methods: {}
}
</script>

<style scoped lang='less'>
.app-container {
  padding: 20px;
  .el-input {
    width: 200px;
    height: 32px;
  }
  .el-button {
    width: 56px;
    height: 36px;
  }
  .el-button:nth-of-type(2) {
    width: 56px;
    height: 36px;
  }
  .el-button:nth-of-type(3) {
    width: 97px;
    height: 36px;
  }
  .card-info {
    margin-bottom: 20px;
  }
  .info {
    width: 1183px;
    height: 35px;
    border-radius: 4px;
    margin: 0 0 10px;
    padding: 8px 16px;
    background-color: #f4f4f5;
    color: #909399;
  }
}
</style>
