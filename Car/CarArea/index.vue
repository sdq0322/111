<template>
  <div class="building-container">
    <!-- 新增操作区域 -->
    <div class="create-container">
      <el-button type="primary">添加区域</el-button>
    </div>
    <!-- 表格区域 -->
    <div class="table">
      <el-table style="width: 100%" :data="list">
        <el-table-column type="index" label="序号" :index="indexMethod"/>
        <el-table-column label="区域名称" prop="areaName" />
        <!-- prop的值还没有改-->
        <el-table-column label="车位数" prop="spaceNumber" />
        <el-table-column label="在管面积" prop="areaProportion" />
        <el-table-column label="备注" prop="remark" />

        <el-table-column label="操作" fixed="right" width="180">
          <template>
            <!-- <el-button size="mini" type="text">续费</el-button>
            <el-button size="mini" type="text">查看</el-button> -->
            <el-button size="mini" type="text">编辑</el-button>
            <el-button size="mini" type="text">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <div class="page-container">
      <!-- current-page 表示当前页码 -->
      <!-- page-sizes 可选择的每页的页容量 -->
      <!-- page-size 当前页的页容量 page-size的值通常和page-sizes的第一个值保持一致-->
      <!-- layout 分页的样式 -->
      <!-- total 一共有多少条 -->
      <!-- @size-change 页容量发生变化时 会触发这个事件 -->
      <!-- @current-change 页码发生变化时 会触发这个事件-->
      <el-pagination
        layout="total, sizes, prev, pager, next, jumper"
        :current-page="params.page"
        :page-sizes="[2, 4, 6, 8]"
        :page-size="params.pageSize"
        :total="total"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </div>
  </div>

</template>
<script>
import { getAreaListAPI } from '@/api/area'
export default {
  name: 'Area',
  // 封装参数
  data() {
    return {
      params: {
        name: '',
        page: 1,
        pageSize: 2
      },
      list: [],
      total: 0
    }
  },
  created() {
    this.getAreaList()
  },
  // 封装接口
  methods: {
    // 计算序号
    indexMethod(index) {
      // 当前页之前所有的数据+index+
      return (this.params.page - 1) * this.params.pageSize + index + 1
    },
    // 获取公寓列表
    async getAreaList() {
      const res = await getAreaListAPI(this.params)
      this.list = res.data.rows
      this.total = res.data.total
    },
    handleSizeChange(val) {
      // 当每页显示条目个数变化时触发
      this.params.pageSize = val
      // 这里可以添加请求数据的逻辑
    },
    handleCurrentChange(val) {
      // 当页码变化时触发
      this.params.page = val
      // 这里可以添加请求数据的逻辑
    }
  }
}
</script>
<style lang="scss" scoped>
.building-container {
  padding: 20px;
  background-color: #fff;
}

.search-container {
  display: flex;
  align-items: center;
  border-bottom: 1px solid rgb(237, 237, 237, .9);
  padding-bottom: 20px;

  .search-main {
    width: 220px;
    margin-right: 10px;
  }

  .search-btn {
    margin-left: 20px;
  }
}

.create-container {
  margin: 10px 0px;
}
.page-container{
  padding:4px 0px;
  text-align: right;
}
</style>

