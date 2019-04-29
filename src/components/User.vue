<template>
  <div>
    <!-- 好了 因为上一个版本bug不断 所以重新开始写了 -->
    <!-- 那么现在开始写的是什么呢  就是除了登陆之外最基本的用户列表   有点刺激
    所以让我们看一看需要写什么   (不写一堆注释难受)
    √1.面包屑导航  用的是饿了么组件  el-breadcrumb  面包屑

    √2.卡片区  用的是el-card  空白区域  要在global.css中写样式

    √3.搜索框和搜索的icon(也就是输入框)用的是复合型输入框el-input

    √4.然后是添加用户的按钮  用的是el-button  可以加上不同的type  变颜色 只不过比较单一 一会可以试一下换换颜色
    ----搜索框和按钮会被分为两行 所以要借助布局来改一下-----41-52行
    ==用的是el-row  和el-col  利用el-col的span属性

    √5.  表格 表格是隔行换色  el-table  添加stripe 属性 可以实现隔行换色

    √ 6.表格里面有个状态开关  用的是 el-switch  没用老师的那个  这个是可以实现换色的  active-color="#13ce66"
    inactive-color="#ff4949  设置开关的颜色  很强
    ------这个地方有个问题 就是如果这里不写逻辑 那么这个按钮是按不动的  所以就要写逻辑  emm  让我寻思寻思怎么写的..........
    ==这里用到的是插槽 因为数据是一行一行的 一般是要用v-for循环去遍历的  但是v-for循环在这里 循环机制我们看不到
    然鹅 el-table 表格自带v-for机制 所以使用作用域插槽来获取数据  之前接触到的作用域插槽 就是 在el-swith标签上
    写一个作用域插槽 slot-scope="xxx" 这个xxx就代表的是接收到的全部数据  emm  还是console.log一下看看8
    emmm  打印不出来 但是只要知道是数据就可以了

    7.表格里还有个按钮  来实现删改 和用户角色管理  老师用的是方的 但是个人更喜欢圆的  用的是el-button  可以更改icon  也是用
    type属性去修改颜色  理论上来说也是可以改颜色的  emm

    8.分页部分  用的是el-pagination  完整分页  加background属性可以更改颜色

    好惹 大概剖析就这样 有遗忘的一会再补充  现在先写样式  先抓一个 样式写完再考虑逻辑
    样式可以说算是写完了8 现在就要开始写逻辑了
    首先的逻辑就是查:展示列表数据
    emm  这个应该是不需要事件触发的8...........页面一刷新就开始?get请求吗...
    根据接口文档 请求的时候 是有三个参数的  query查询参数,可以为空
    pagenum 当前页码 不能为空
    pagesize每页显示条数  不能为空  接口是users
    上面的几个数据记得在下面data里写
    el-table 上的:data  是来接收数据的  不要删了..
    -->
    <!-- 面包屑开始 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 面包屑导航结束 -->
    <!-- 卡片开始 -->
    <el-card class="box-card">
      <div class="text item">
        <!-- 输入搜索框开始 -->
        <!-- 可以给行加上属性gutter 设定每一列之间的宽度 -->
        <el-row :gutter="20">
          <el-col :span="8">
            <el-input placeholder="请输入内容" class="input-with-select">
              <el-button slot="append" icon="el-icon-search"></el-button>
            </el-input>
            <!-- 输入搜索框结束 -->
          </el-col>
          <el-col :span="8">
            <!-- 添加按钮开始 -->
            <el-button id="addBtn">添加用户</el-button>
            <!-- 添加按钮结束 -->
            <!-- 现在的情况是 搜索框和按钮被分为两行了 所以就要使用布局 给他们安排一下 -->
          </el-col>
        </el-row>
        <!-- 表格开始 -->
        <el-table :data="userList" stripe style="width: 100%" border>
          <!-- 这个type=index是自定义的列的的序号自定义索引
          自定义 type=index 列的行号。-->
          <el-table-column type="index" label="序号" width="80"></el-table-column>
          <el-table-column prop="username" label="用户名" width></el-table-column>
          <el-table-column prop="mobile" label="手机号" width="180"></el-table-column>
          <el-table-column prop="role_name" label="角色" width="160"></el-table-column>
          <el-table-column prop="email" label="邮箱" width="160"></el-table-column>
          <el-table-column prop="mg_state" label="状态" width="160">
            <!-- 状态开始 -->
            <el-switch
              active-color="#FFCCCc"
              inactive-color="#ccc"
              slot-scope="info"
              v-model="info.row.mg_state"
            >
              <!-- 因为看不到v-for的循环信息 所以就只能借助插槽 来接收一下每一行的数据
              -->
            </el-switch>
            <!-- 状态结束 -->
          </el-table-column>
          <!-- 操作开始 -->
          <el-table-column prop="address" label="操作" width="220">
            <el-button type="primary" icon="el-icon-edit" circle></el-button>
            <el-button type="danger" icon="el-icon-delete" circle></el-button>
            <el-button type="warning" icon="el-icon-setting" circle></el-button>
          </el-table-column>
        </el-table>
        <!-- 表格结束 -->
        <!-- 分页开始 -->
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :page-sizes="[3, 5, 10, 20]"
          :page-size="3"
          layout="total, sizes, prev, pager, next, jumper"
          :total="400"
        ></el-pagination>
        <!-- 分页结束 -->
      </div>
    </el-card>
    <!-- 卡片结束 -->
  </div>
</template>

<script>
export default {
  // 虽然我也不知道为什么 但是要生命周期函数 (以为要调用 之前在node的时候 都是直接页面一加载就触发get请求 但是这个是要去
  // 调用的 但是有没地方可以让她调用  只能在生命周期中调用   其实这里还是很懵逼的 TODO)
  created() {
    this.getUserList()
  },
  data() {
    return {
      s_UserList: {
        query: '',
        pagenum: '1',
        // 从第一页开始显示
        pagesize: '3'
        // 每一页显示的条数
      },
      userList: []
      //   emm  不能数据返回来了不用东西去接收8  所以就定义了一个data属性来
      // 接收 在数据检索 分页的时候都需要  emm 虽然我也不知道为啥
      // 就姑且那么写8
    }
  },
  methods: {
    // 展示用户数据开始
    async getUserList() {
      const { data: dt } = await this.$http.get('/users', {
        params: this.s_UserList
      })
      console.log(dt)
      // 因为要接收一下 目前看来是data里面还有data  所以就解构赋值一下
      // 判断一下 如果接收成功 200 就让userList=dt.data
      if (dt.meta.status === 200) {
        this.userList = dt.data.users
      }
      // console.log(this.info)
    },
    // 分页相关
    handleSizeChange() {},
    handleCurrentChange() {}
  }
}
</script>
<style lang="less" scoped>
</style>
