<template>
  <div id = 'app'>
    <el-container >
      <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <el-menu :default-openeds="['0',]">
          <el-submenu index="i" v-for="(item,i) in aside" :key="i" >
            <template slot="title">{{item.cate}}</template>
            <el-menu-item-group>
              <el-menu-item index="1-1" v-for="(subItem,j) in item.items " :key="j">
                <a href="#" @click="clickSideItem(subItem.route,subItem.title)">{{subItem.title}}</a>
              </el-menu-item>
            </el-menu-item-group>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-container>
        <el-header style="text-align: right; font-size: 12px">
          <el-input disabled v-text="currentTitle" style="text-align: left; font-size: 20px;width: 70%;" ></el-input>
          <el-dropdown>
            <i class="el-icon-view" style="margin-right: 15px"></i>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item>查看</el-dropdown-item>
              <el-dropdown-item>新增</el-dropdown-item>
              <el-dropdown-item>删除</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>

        </el-header>

        <el-main>
          <el-table :data="tableData">
            <el-table-column prop="date" label="日期" width="140">
            </el-table-column>
            <el-table-column prop="name" label="姓名" width="120">
            </el-table-column>
            <el-table-column prop="address" label="地址">
            </el-table-column>
          </el-table>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {
  },
  data() {
    const item = {
      date: '2016-05-02',
      name: '王小虎',
      address: '上海市普陀区金沙江路 1518 弄'
    };
    return {
      tableData: Array(20).fill(item),
      aside: [
        {
          cate:'webgl',
          items:
            [
              {title: '1.webgl三角形',route: '1'},
              {title: '2.webgl平移',route: '1'}
            ]
        },
        {
          cate:'three',
          items:
            [
              {title: '1.three',route: '1'},
              {title: '2.three',route: '1'}
            ]
        }
      ],
      currentTitle: '123'
    }
  },
  methods:{
    clickSideItem (route, title){
      this.currentTitle = title
    },
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0px;
}
.el-header {
  background-color: #B3C0D1;
  color: #333;
  line-height: 60px;
}

.el-aside {
  color: #333;
}
</style>
