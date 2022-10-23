<template>
  <div>
    <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-col :span="6">
        <el-form-item label="标签名称" label-width="80px">
          <el-input v-model="formInline.tags" placeholder="请输入"></el-input>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item label="城市" label-width="80px" @click="getCity">
          <el-select v-model="formInline.province" placeholder="请选择">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item label="地区" label-width="80px">
          <el-select v-model="formInline.city" placeholder="请选择">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item label="企业简称">
          <el-input v-model="formInline.shortName"></el-input>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item label="状态" label-width="80px">
          <el-select v-model="formInline.state" placeholder="请选择">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item style="padding-left: 40px">
          <el-button>清除</el-button>
          <el-button type="primary" @click="onSubmit">搜索</el-button>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-button type="success">
          <i class="el-icon-edit"></i>
          <span>新增用户</span>
        </el-button>
      </el-col>
    </el-form>
  </div>
</template>

<script>
export default {
  name: 'articlesSearch'
}
</script>

<style>
</style>
