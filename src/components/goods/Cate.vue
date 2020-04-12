<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>商品管理</el-breadcrumb-item>
      <el-breadcrumb-item>商品分类</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片视图 -->
    <el-card>
      <el-row>
        <el-col>
          <el-button type="primary">添加分类</el-button>
        </el-col>
      </el-row>
      <!-- 表格 -->
      <tree-table
        :data="catelist"
        :columns="columns"
        :selection-type="false"
        :show-index="true"
        :expand-type="false"
        index-text="#"
        :show-row-hover="false"
      >
        <template slot="isok" slot-scope="scope">
          <i v-if="scope.row.cat_deleted===false" class="el-icon-success" style="color:lightgreen"></i>
          <i v-else class="el-icon-error" style="color:red"></i>
        </template>
        <template slot="level" slot-scope="scope">
          <el-tag v-if="scope.row.cat_level===0" size="mini">一级</el-tag>
          <el-tag v-else-if="scope.row.cat_level===1" type="success" size="mini">二级</el-tag>
          <el-tag v-else type="warning" size="mini">三级</el-tag>
        </template>
        <template slot="opt">
          <el-button type="primary" icon="el-icon-deit" size="mini">编辑</el-button>
          <el-button type="danger" icon="el-icon-delete" size="mini">删除</el-button>
        </template>
      </tree-table>
      <!-- 分页 -->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="queryInfo.pagenum"
        :page-sizes="[3, 5, 10, 15]"
        :page-size="queryInfo.pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      ></el-pagination>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 商品分类数据表
      catelist: [],
      //   查询条件
      queryInfo: {
        type: 3,
        pagenum: 1,
        pagesize: 5
      },
      //   总数据条数
      total: 0,
      columns: [
        {
          label: '分类名称',
          prop: 'cat_name',
          width: '400px'
        },
        {
          label: '是否有效',
          // 将当前列定义为模板列
          type: 'template',
          // 当前列模板名称
          template: 'isok'
        },
        {
          label: '排序',
          type: 'template',
          template: 'level'
        },
        {
          label: '操作',
          type: 'template',
          template: 'opt'
        }
      ]
    }
  },
  created() {
    this.getCategoriesList()
  },
  methods: {
    async getCategoriesList() {
      const { data: res } = await this.$http.get('categories', {
        params: this.queryInfo.type
      })
      if (res.meta.status !== 200) {
        return this.$message('获取商品分类数据失败！')
      }
      console.log(res)
      this.catelist = res.data
      this.total = res.data.length
    },
    //   监听pagesize变化
    handleSizeChange(newSize) {
      this.queryInfo.pagesize = newSize
      this.getCategoriesList()
    },
    //   监听pagenum变化
    handleCurrentChange(newPage) {
      this.queryInfo.pagenum = newPage
      this.getCategoriesList()
    }
  }
}
</script>

<style scoped>
</style>
