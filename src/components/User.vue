<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>
    <el-card class="box-card">
      <div>
        <!-- {{'列表内容 ' + o }} -->
        <el-row type="flex"
                class="row-bg"
                :gutter="20">
          <el-col :span="8">
            <el-input placeholder="请输入内容"
                      v-model="input3"
                      class="input-with-select">
              <el-button slot="append"
                         icon="el-icon-search"></el-button>
            </el-input>
          </el-col>
          <el-col :span="8 ">
            <el-button type="primary"
                       id="add">添加用户</el-button>
          </el-col>
        </el-row>

        <el-table
    :data="tableData"
    height="250"
    border
    style="width: 100%"
    id="table">
    <el-table-column
      prop="num"
      label="序号"
      width="80">
    </el-table-column>
    <el-table-column
      prop="name"
      label="用户名"
      >
    </el-table-column>
    <el-table-column
      prop=""
      label="手机号码"
      width="180">
    </el-table-column>
     <el-table-column
      prop=""
      label="角色"
      width="160">
    </el-table-column>
     <el-table-column
      prop=""
      label="邮箱"
      width="160">
    </el-table-column>
     <el-table-column
      prop=""
      label="状态"
      width="160">
    </el-table-column>
     <el-table-column
      prop=""
      label="操作"
      width="200">
    </el-table-column>
  </el-table>
  <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage4"
      :page-sizes="[3, 5, 10, 20]"
      :page-size="3"
      layout="total, sizes, prev, pager, next, jumper"
      :total=""
      id="page">
    </el-pagination>
      </div>
    </el-card>
  </div>
</template>

<script>
export default {}
</script>
<style lang="less" scoped>
</style>
