<template>
  <el-dialog
    title="新增学科"
    :visible="dialogTableVisible"
    @close="onClose"
    width="20%"
  >
    <el-form ref="form" :model="form" label-width="80px">
      <el-form-item label="所属学科">
        <el-select v-model="form.subjectID" placeholder="请选择" width="100%">
          <el-option
            v-for="item in options"
            :key="item.id"
            :label="item.subjectName"
            :value="item.id"
          >
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="目录名称">
        <el-input v-model="form.tagName" placeholder="请输入内容"></el-input>
      </el-form-item>
      <el-button size="small" class="btn">取消</el-button>
      <el-button size="small" type="primary" @click="sumbit">确认</el-button>
    </el-form>
  </el-dialog>
</template>

<script>
import { add, update } from '@/api/hmmm/tags'
import { list } from '@/api/hmmm/subjects'

export default {
  props: {
    dialogTableVisible: {
      type: Boolean,
      require: true
    },
    form: {
      type: Object,
      default: function () {
        return {
          subjectName: '',
          value: ''
        }
      }
    }
  },
  created () {
    this.list()
  },
  data () {
    return {
      options: []
    }
  },
  methods: {
    async list () {
      const { data } = await list()
      this.options = data.items
      console.log(this.options)
    },
    onClose () {
      console.log(this.$refs)
      this.$emit('update:form', {
        subjectID: '',
        value: ''
      })
      this.$emit('update:dialogTableVisible', false)
    },
    async sumbit () {
      try {
        if (this.form.id) {
          await update(this.form)
          console.log(1)
          this.$message.success('修改成功')
        } else {
          await add(this.form)
          this.$message.success('添加成功')
        }
        this.onClose()
        // console.log(this.form)
        this.$emit('sx')
      } catch (e) {
        console.log(e)
      }
    }
  }
}
</script>

<style>
</style>
