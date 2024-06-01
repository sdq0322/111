<template>
  <div class="pole-container">
    <!-- 搜索区域 -->
    <div class="search-container">
      <span class="search-label">教师姓名：</span>
      <el-input v-model="params.poleName" clearable placeholder="请输入内容" class="search-main" />
      <span class="search-label">所属院系：</span>
      <el-input v-model="params.poleNumber" clearable placeholder="请输入内容" class="search-main" />
      <!-- <span class="search-label">状态：</span>
      <el-select v-model="params.cardStatus"> -->
        <!-- <el-option v-for="item in statusList" :key="item.value" :label="item.text" :value="item.value" /> -->
      <!-- </el-select> -->
      <el-button type="primary" class="search-btn" @click="getPoleList">查询</el-button>
    </div>
    <!-- 表格区域 -->
    <div class="table">
      <el-table style="width: 100%" :data="list">
        <el-table-column type="index" label="序号" :index="indexMethod" />
        <el-table-column label="教师姓名" prop="poleName" />
        <el-table-column label="所属院系" prop="poleNumber" />
        <el-table-column label="所属系部" prop="poleIp" />

        <el-table-column label="主授课程" prop="areaId" />
        <el-table-column label="科研方向" prop="poleType" />
        <el-table-column label="在研课题" prop="poleStatus" :formatter="formatStatus" />
        <el-table-column label="操作" fixed="right" width="180">
          <template>
            <!-- <el-button size="mini" type="text">续费</el-button> -->
            <!-- <el-button size="mini" type="text">查看</el-button> -->
            <!-- editCard(scope.row.id)作用域插槽获取当前行的数据 -->
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
import { getPoleListAPI } from '@/api/pole'
export default {
  name: 'Pole',
  data() {
    return {
      params: {
        page: 1,
        pageSize: 4
      },
      list: [],
      total: 0
    }
  },
  created() {
    this.getPoleList()
  },
  methods: {
    // 计算序号
    indexMethod(index) {
      // 当前页之前所有的数据+index+
      return (this.params.page - 1) * this.params.pageSize + index + 1
    },
    // 格式化状态
    formatStatus(row, column, cellValue, index) {
      // if (cellValue === 0) {
      //   return '可用'
      // } else {
      //   return '不可用'
      // }
      const MAP = {
        0: '可用',
        1: '不可用'
      }
      return MAP[cellValue]
    },
    // 获取教职工列表
    async getPoleList() {
      const res = await getPoleListAPI(this.params)
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
.card-container {
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

.page-container {
  padding: 4px 0px;
  text-align: right;
}

.form-container {
  padding: 0px 80px;
}
</style>

