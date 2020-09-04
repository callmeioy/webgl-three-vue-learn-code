<template>
  <el-container  id = 'app'>
    <!--侧栏-->
    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
      <el-menu :default-openeds="[0]">
        <el-submenu  v-for="(item,i) in aside"  :index = 'item.cate'  :key="i" >
          <template slot="title">{{item.cate}}</template>
          <el-menu-item-group>
            <el-menu-item v-for="(subItem,j) in item.items " :index = "subItem.com"  :key="j">
              <a href="#" @click="clickSideItem(subItem.title, subItem.com)">{{subItem.title}}</a>
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

      <!--展示 main-->
      <el-main :is = "filterComponent"></el-main>
    </el-container>


  </el-container>


</template>
<script>
  import * as components from './components/components.js'
  export default {
    name: "App",
    components,
    data() {
      return {
        aside: [
          {
            cate:'webgl',
            items:
              [
                {title: '1.webgl程序', com:'w1-hello-world'},
                {title: '2.webgl图形', com:'w2-square'},
                {title: '3.webgl给顶点着色', com:'w3-shader-color'},
                {title: '4.webgl循环渲染和空间变化', com:'w4-animate'},
                {title: '5.webgl立方体', com:'w5-cube'},
              ]
          },
          {
            cate:'three',
            items:
              [
                {title: '1.three', com:'t1-hello-world'},
                // {title: '2.three',route: '1'}
              ]
          },
          {
            cate:'cesium',
            items:
              [
                {title: '1.cesium', com:'c1-hello-world'},
                // {title: '2.cesium',route: '1'}
              ]
          },
          // {
          //   cate:'babyicon',
          //   items:
          //           [
          //             {title: '1.babyicon', com:'c1-hello-word'},
          //             // {title: '2.babyicon',route: '1'}
          //           ]
          // }
        ],
        currentTitle: '',
        filterComponent: ''
      }
    },
    methods:{
      clickSideItem (title,com){
        this.currentTitle = title
        this.filterComponent = com
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
