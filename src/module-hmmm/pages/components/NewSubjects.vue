<template>
  <el-dialog title="新增学科" :visible="dialogTableVisible" @close="onClose">
    <el-form ref="form" :model="form" label-width="80px">
      <el-form-item label="学科名称">
        <el-input
          v-model="form.subjectName"
          placeholder="请输入内容"
        ></el-input>
      </el-form-item>
      <el-form-item label="是否显示">
        <el-switch
          v-model="form.isFrontDisplay"
          active-color="#13ce66"
          inactive-color="#ff4949"
          :active-value="1"
          :inactive-value="0"
        >
        </el-switch>
      </el-form-item>
      <el-button size="small" class="btn">取消</el-button>
      <el-button size="small" type="primary" @click="sumbit">确认</el-button>
    </el-form>
  </el-dialog>
</template>

<script>
import { add, update } from '@/api/hmmm/subjects'

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
          isFrontDisplay: 1
        }
      }
    }
  },
  data () {
    return {

    }
  },
  methods: {
    onClose () {
      this.$emit('update:form', {
        subjectName: '',
        isFrontDisplay: 1
      })
      this.$emit('update:dialogTableVisible', false)
    },
    async sumbit () {
      try {
        if (this.form.id) {
          await update(this.form)
          this.$message.success('修改成功')
        } else {
          await add(this.form)
          this.$message.success('添加成功')
        }
        this.onClose()
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
