<template>
  <div class="building-container">
    <!-- 搜索区域 -->
    <div class="search-container">
      <div class="search-label">公寓名称：</div>
      <el-input clearable placeholder="请输入内容" class="search-main" v-model="params.name" @clear="search"/>
      <el-button type="primary" @click="search">查询</el-button>
    </div>
    <div class="create-container">
      <el-button type="primary"  @click="$router.push('/AddEnterprise')">添加企业</el-button>
    </div>
    <!-- 表格区域 -->
    <div class="table">
      <el-table style="width: 100%" :data="list" >
        <el-table-column type="index" label="序号" :index="indexMethod"/>
        <el-table-column label="公寓名称" width="320" prop="name" />
        <el-table-column label="所属班级" prop="contact" />
        <el-table-column
          label="联系电话"
          prop="contactNumber"
        />
        <el-table-column label="操作">
          <template #default="scope">
            <!-- <el-button size="mini" type="text">添加合同</el-button> -->
            <!-- <el-button size="mini" type="text">查看</el-button> -->
            <el-button size="mini" type="text" @click="toEditPage(scope.row.id)">编辑</el-button>
            <el-button size="mini" type="text" @click="delEnterprise(scope.row.id)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <div class="page-container">
      <!-- total和page-size是必须要配置的 总数=total/page-size -->
      <!-- @current-change 必须要配置的 切换页码用的 -->
      <el-pagination
        layout="total, prev, pager, next"
        :total="total"
        :page-size="params.pageSize"
        @current-change="handleChange"
      />
    </div>
  </div>
</template>

<script>
import {getEnterpriseListAPI ,delEnterpriseAPI} from "@/api/enterprise"
export default {
  name:'Enterprise',
  data(){
    return {
      params:{
        name:'',
        page:1,
        pageSize:2
      },
    list:[],
    total:0
    }
},
  created(){
    this.getEnterpriseList()
  },

  methods:{
    // 删除企业
    delEnterprise(id){
      this.$confirm('您确定要删除该数据吗？','温馨提示').then(async ()=>{
        await delEnterpriseAPI(id)
        this.$message.success('删除成功')
        if(this.list.length===1&&this.params.page>1){
          this.params.page--
        }
        this.getEnterpriseList()
      }).catch(()=>{});
    },
    toEditPage(id){
      this.$router.push({
        path:'/addEnterprise',
        query:{
          id
        }
      })
    },

    // 清空
    clearSearch(){

    },
    // 点击查询按钮
    search(){
      this.params.page = 1
      this.getEnterpriseList()
    },
    // 计算序号
    indexMethod(index){
      return (this.params.page-1) *this.params.pageSize + index+1
    },
    // 点击分页
    handleChange(val){
      this.params.page = val
      this.getEnterpriseList()
    },
    // 获取列表
    async getEnterpriseList(){
      const res =  await getEnterpriseListAPI(this.params)
      this.list = res.data.rows
      this.total = res.data.total
    },
  }
}
</script>


<style lang="scss" scoped>
.department-container {
  padding: 10px;
}

.search-container {
  display: flex;
  align-items: center;
  border-bottom: 1px solid rgb(237, 237, 237, .9);
  ;
  padding-bottom: 20px;

  .search-label {
    width: 100px;
  }

  .search-main {
    width: 220px;
    margin-right: 10px;
  }
}
.create-container{
  margin: 10px 0px;
}
.page-container{
  padding:4px 0px;
  text-align: right;
}
.form-container{
  padding:0px 80px;
}
</style>