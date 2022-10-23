<template>
  <div class="container">
    目录管理
    <el-button @click="ab"> </el-button>
  </div>
</template>

<script>
import { status } from '../../api/hmmm/constants'
export default {
  methods: {
    ab () {
      status.value = 0
    }
  }
}
</script>

<style scoped lang='less'></style>
