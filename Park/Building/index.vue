<!-- eslint-disable vue/valid-template-root -->
<template>
  <div class="building-container">
    <!-- 搜索区域 -->
    <div class="search-container">
      <span class="search-label">学院名称：</span>
      <el-input clearable placeholder="请输入学院名称" class="search-main" />
      <!-- <el-select v-model="params.cardStatus">
        <el-option v-for="item in []" :key="item.id" />
      </el-select> -->
      <el-button type="primary" class="search-btn">查询</el-button>
    </div>
    <!-- 新增操作区域 -->
    <div class="create-container">
      <el-button type="primary">添加学院</el-button>
    </div>
    <!-- 表格区域 -->
    <div class="table">
      <el-table style="width: 100%" :data="list">
        <el-table-column type="index" label="序号" />
        <el-table-column label="学院名称" prop="name" />
        <!-- prop的值还没有改-->
        <el-table-column label="单位系部" prop="floors" />
        <el-table-column label="所在位置" prop="area" />
        <el-table-column label="房间名称" prop="propertyFeePrice" />
        <el-table-column label="简介" prop="status" />

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
import { getBuildingListAPI } from '@/api/building'
export default {
  name: 'Building',
  // 封装参数
  data() {
    return {
      params: {
        name: '',
        page: 1,
        pageSize: 10
      },
      list: [],
      total: 0
    }
  },
  created() {
    this.getBuildingList()
  },
  // 封装接口
  methods: {
    // 计算序号
    indexMethod(index) {
      // 当前页之前所有的数据+index+
      return (this.params.page - 1) * this.params.pageSize + index + 1
    },
    // 获取公寓列表
    async getBuildingList() {
      const res = await getBuildingListAPI(this.params)
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

