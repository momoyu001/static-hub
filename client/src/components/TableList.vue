<template>
  <div class="table-list-module">
    <el-table :data="data" :border="true" style="width: 100%" @row-click="handleRowClick">
      <el-table-column prop="id" label="编号"></el-table-column>
      <el-table-column prop="name" label="名称"></el-table-column>
      <el-table-column prop="url" label="地址"></el-table-column>
      <el-table-column prop="note" label="备注"></el-table-column>
      <el-table-column prop="preview" label="预览">
        <template #default="scope">
          <img
            class="preview-img"
            :src="scope.row.preview"
            alt="图片预览"
            srcset=""
            @click="handlePreview(scope.row.preview)"
          />
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      class="pagination"
      layout="prev, pager, next, ->, total"
      :total="total"
      @prev-click="preClick"
      @next-click="nextClick"
      @current-change="currentChange"
    ></el-pagination>

    <el-dialog v-model="dialogVisible" title="图片预览" width="600px">
      <img class="img" w-full :src="dialogImageUrl" alt="Preview Image" />
    </el-dialog>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { useRequest } from '@/utils/use-request'

const { $axios } = useRequest()

interface dataType {
  id: number
  name: string
  url: string
  note: string
  preview: string
}

interface Props {
  data: dataType[]
  total: number
}

let dialogImageUrl = ref('')
let dialogVisible = ref(false)

withDefaults(defineProps<Props>(), {
  data: () => []
})

function preClick(value: number) {
  console.log('页码：', value)
}

function nextClick(value: number) {
  console.log('页码：', value)
}

function currentChange(value: number) {
  console.log('页码：', value)
}

function handlePreview(url: string) {
  dialogImageUrl.value = url
  dialogVisible.value = true
}

async function handleRowClick(row: any) {
  const result = await $axios.get(`/img/${row.id}`)
  console.log(result)
}
</script>

<style scoped lang="less">
@border-color: #c4c8ca;
.table-list-module {
  .pagination {
    margin-top: 20px;
  }
  .preview-img {
    width: 100px;
    height: 60px;
    cursor: pointer;
  }

  /deep/ .el-dialog__header {
    height: 54px;
  }

  .img {
    width: 550px;
    margin: 0px auto;
  }
}
</style>
