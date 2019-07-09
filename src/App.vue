<template>
  <div id = 'app'>
    <el-container >
      <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <el-menu :default-openeds="['0',]">
          <el-submenu index="i" v-for="(item,i) in aside" :key="i" >
            <template slot="title">{{item.cate}}</template>
            <el-menu-item-group>
              <el-menu-item index="1-1" v-for="(subItem,j) in item.items " :key="j">
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

        <el-main>
          <div :is = "filterComponent"></div>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>
<script>
  import C1HelloWorld from './components/cesium/C1HelloWorld'
  import T1HelloWorld from './components/three/T1HelloWorld'
  import W1HelloWorld from './components/webgl/W1HelloWorld'
  export default {
    name: "App",
    components: {
      C1HelloWorld,T1HelloWorld,W1HelloWorld
    },
    data() {
      return {
        aside: [
          {
            cate:'webgl',
            items:
              [
                {title: '1.webgl三角形',route: '1', com:'w1-hello-world'},
                // {title: '2.webgl平移',route: '1'}
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
          {
            cate:'babyicon',
            items:
                    [
                      {title: '1.babyicon', com:'c1-hello-word'},
                      // {title: '2.babyicon',route: '1'}
                    ]
          }
        ],
        currentTitle: '123',
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
