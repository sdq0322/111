
<template>
  <div class="card-container">
    <!-- 搜索区域 -->
    <div class="search-container">
      <span class="search-label">车牌号码：</span>
      <el-input clearable placeholder="请输入车牌号码" class="search-main" />
      <span class="search-label">缴纳状态：</span>
      <el-input clearable placeholder="未选择" class="search-main" />
      <el-button type="primary" class="search-btn">查询</el-button>
    </div>
    <!-- 表格区域 -->
    <div class="table">
      <el-table style="width: 100%" :data="list">
        <el-table-column type="index" label="序号" :index="indexMethod" />
        <el-table-column label="车主名称" prop="personName" />
        <el-table-column label="联系方式" prop="phoneNumber" />
        <el-table-column label="车牌号码" prop="carNumber" />
        <!-- <el-table-column label="车辆品牌" prop="carBrand" /> -->
        <!-- <el-table-column label="剩余有效天数" prop="totalEffectiveDate" /> -->
        <el-table-column label="是否注册" prop="cardStatus" :formatter="formatStatus" />
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
    <!-- 添加楼宇 -->
    <el-dialog title="添加楼宇" width="580px">
      <!-- 表单接口 -->
      <div class="form-container">
        <!-- <el-form ref="addForm" :model="addForm" :rules="addFormRules">
          <el-form-item label="楼宇名称" prop="name">
            <el-input v-model="addForm.name" />
          </el-form-item>
          <el-form-item label="楼宇层数" prop="floors">
            <el-input v-model="addForm.floors" />
          </el-form-item>
          <el-form-item label="在管面积" prop="area">
            <el-input v-model="addForm.area" />
          </el-form-item>
          <el-form-item label="物业费" prop="propertyFeePrice">
            <el-input v-model="addForm.propertyFeePrice" />
          </el-form-item>
        </el-form> -->
      </div>
      <template #footer>
        <el-button size="mini">取 消</el-button>
        <el-button size="mini" type="primary">确 定</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script>
import { getCardListAPI } from '@/api/card'
export default {
  name: 'Card',
  data() {
    return {
      params: {
        page: 1,
        pageSize: 2,
        carNumber: '',
        personName: '',
        cardStatus: null
      },
      list: [],
      total: 0
    }
  },
  created() {
    this.getCardList()
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
        0: '是',
        1: '否'
      }
      return MAP[cellValue]
    },
    // 获取月卡列表
    async getCardList() {
      const res = await getCardListAPI(this.params)
      this.list = res.data.rows
      this.total = res.data.total
    },
    handleSizeChange(val) {
      this.params.pageSize = val
      this.getCardList()
    },
    handleCurrentChange(val) {
      this.params.page = val
      this.getCardList()
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
