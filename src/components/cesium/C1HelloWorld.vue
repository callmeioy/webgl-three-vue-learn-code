<template>
  <div>
    <el-progress :text-inside="true" :stroke-width="26" :percentage="percentage"></el-progress>
    <el-progress :percentage="percentage"></el-progress>
    <el-progress type="circle" :percentage="percentage"></el-progress>

    <el-button-group>
      <el-button icon="el-icon-minus" @click="decrease"></el-button>
      <el-button icon="el-icon-plus" @click="increase"></el-button>
     <input v-model="percentage"/>
     <button v-bind:disabled="isButtonDisabled">Button</button>
    </el-button-group>

    <el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column
        prop="date"
        label="日期"
        width="180">
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
        width="180">
      </el-table-column>
      <el-table-column
        prop="address"
        label="地址">
      </el-table-column>
      <el-table-column
        prop="percentage"
        label="进度">
         <el-progress :text-inside="true" :stroke-width="26" :percentage="percentage"></el-progress>
      </el-table-column>
    </el-table>

  </div>
</template>

<script>
export default {
  name: "C1HelloWorld",
  data() {
    return {
      percentage: 20,
      isButtonDisabled:false,
      tableData: [{
            date: '2016-05-02',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1518 弄',
            percentage:0
          }, {
            date: '2016-05-04',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1517 弄',
            percentage:0
          }, {
            date: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1519 弄',
            percentage:0
          }, {
            date: '2016-05-03',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1516 弄',
            percentage:0
          }]
    };
  },
  methods: {
    increase() {
      this.percentage += 10;
      if (this.percentage > 100) {
        this.percentage = 100;
      }
    },
    decrease() {
      this.percentage -= 10;
      if (this.percentage < 0) {
        this.percentage = 0;
      }
    }
  }
};
</script>

<style scoped>
</style>
