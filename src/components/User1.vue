<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>

    <!--添加用户 对话框-->
    <el-dialog title="添加用户" :visible.sync="addUserDialog" width="50%">
      <span>这是一段信息</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="addUserDialog = false">取 消</el-button>
        <el-button type="primary" @click="addUserDialog = false">确 定</el-button>
      </span>
    </el-dialog>

    <!--卡片区-->
    <el-card class="box-card">
      <!--搜索框和添加按钮-->
      <el-row :gutter="20">
        <el-col :span="8">
          <el-input
            placeholder="请输入搜索内容"
            v-model="querycdt.query"
            class="input-with-select"
            clearable
            @clear="search()"
            @keyup.enter.native="search()"
          >
            <el-button slot="append" icon="el-icon-search" @click="search()"></el-button>
          </el-input>
        </el-col>
        <el-col :span="8">
          <el-button type="primary" @click="addUserDialog=true">添加用户</el-button>
        </el-col>
      </el-row>

      <!--数据列表区域-->
      <el-table :data="userList" style="width: 100%" border stripe>
        <el-table-column type="index" label="序号" width="60"></el-table-column>
        <el-table-column prop="username" label="用户名"></el-table-column>
        <el-table-column prop="mobile" label="手机号码" width="140"></el-table-column>
        <el-table-column prop="role_name" label="角色" width="140"></el-table-column>
        <el-table-column prop="email" label="邮箱" width="160"></el-table-column>
        <el-table-column prop="mg_state" label="状态" width="60">
          <!--
            在该处要显示多条记录的不同状态，有的是激活的，有的是未激活的
            我们需要在此处把“每条”用户的记录信息都获取到，然后获取每个用户的状态信息
            现在难在我们看不到v-for遍历的机制代码
            具体语法为：
            <标签 slot-scope="xxxx"></标签>
            xxxx就是代表当前el-table遍历出来的每个用户的对象信息，但是需要xxxx.row的方式获取
            是“作用域插槽”的用法
          -->
          <!--<span slot-scope="info">
            {{ info.row }}  // { "id": 500, "role_name": "超级管理员", "username": "admin", "create_time": 1486720211, "mobile": "12345678", "email": "adsfad@qq.com", "mg_state": true }
          </span>-->
          <!--
            上述span是要给当前组件el-table-column的"插槽"中填充的
            el-table-column组件中有给自己的插槽定义要使用的数据
              <slot row="当前每条记录信息的对象"></slot>
          -->
          <el-switch v-model="info.row.mg_state" slot-scope="info"></el-switch>
        </el-table-column>
        <el-table-column label="操作" width="300">
          <el-button type="primary" icon="el-icon-edit" size="mini"></el-button>
          <el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
          <el-tooltip class="item" effect="dark" content="分配角色" placement="top" :enterable="false">
            <el-button type="warning" icon="el-icon-setting" size="mini"></el-button>
          </el-tooltip>
        </el-table-column>
      </el-table>

      <!--分页-->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="querycdt.pagenum"
        :page-sizes="[3, 5, 10, 20]"
        :page-size="3"
        layout="total, sizes, prev, pager, next, jumper"
        :total="tot"
      ></el-pagination>
    </el-card>
  </div>
</template>

<script>
export default {
  // 生命周期函数
  created() {
    this.getUserList()
  },
  methods: {
    // 数据检索
    search() {
      this.getUserList()
    },
    /** 数据分页相关1 */
    // 当前页码变化的处理
    handleCurrentChange(val) {
      // val变化后的当前页码
      // 把变化后页码赋予给querycdt.pagenum
      this.querycdt.pagenum = val
      // 根据变化页码重新获取数据
      this.getUserList()
    },
    // 每页数据显示条数变化的处理
    handleSizeChange(val) {
      // val：代表当前改变后每页显示的条数
      // console.log(val)
      // 把变化后的显示条数直接赋予给querycdt.pagesize
      this.querycdt.pagesize = val
      // 根据变化后的每页条数重新获取数据
      this.getUserList()
    },
    /** 数据分页相关2 */

    // 获取用户列表数据
    async getUserList() {
      const { data: dt } = await this.$http.get('/users', {
        params: this.querycdt
      })
      console.log(dt)
      // 获取数据失败处理
      if (dt.meta.status !== 200) {
        return this.$message.error(dt.meta.msg)
      }
      // 把获取到的数据总条数赋予给tot存储
      this.tot = dt.data.total
      // 把获得到的数据传递给userList成员
      this.userList = dt.data.users
    }
  },
  data() {
    return {
      /** 添加用户相关1 */
      // 对话框是否显示的标志
      addUserDialog: false,
      /** 添加用户相关2 */
      // 数据记录总条数
      tot: 0,
      // 用户列表数据成员
      userList: [],
      // 获取用户列表需要的参数部分
      // 该参数 在做数据 检索 和 分页  的时候都需要使用
      querycdt: {
        query: '', // 被查询的关键字[数据检索]
        pagenum: 1, // 被查询的页码，默认查询第1页数据[分页]
        pagesize: 3 // 每页显示的记录条数[分页]
      }
    }
  }
}
</script>

<style lang="less" scoped>
</style>
