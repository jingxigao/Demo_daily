<template>
  <div>
    <el-row>
      <el-input placeholder="请输入行数" v-model="rowNum"></el-input>
      <el-input placeholder="请输入列数" v-model="columnNum"></el-input>
      <el-button type="primary" size="small" @click="generateTable">生成表格</el-button>
    </el-row>
    <div class="list-content">
      <el-table :data="tableData2" @cell-click="handle">
        <tr v-for="(col,index) in propsNew" :key="index">
          <el-table-column :label="col" :prop="col" align="center"></el-table-column>-->
        </tr>
      </el-table>
    </div>
  </div>
</template>
<script>
export default {
  name: "table-new",
  data() {
    return {
      rowNum: 0,
      columnNum: 0,
      showForm: false,
      tableData2: [],
      propsNew: [],
      tableData: [
        {
          n1: 11,
          n2: 22,
          n3: 33
        },
        {
          n1: 21,
          n2: 22,
          n3: 23
        },
        {
          n1: 31,
          n2: 32,
          n3: 33
        },
        {
          n1: 41,
          n2: 42,
          n3: 43
        }
      ]
    };
  },
  methods: {
    generateTable() {
      for (var i = 0; i < this.columnNum; i++) {
        var rowX = "Column";
        this.propsNew.push(rowX);
      }
      for (var j = 0; j < this.rowNum; j++) {
        var tempObj = {};
        // eval("obj.p" + j + "=" + j);
        for (var i = 0; i < this.columnNum; i++) {
          tempObj[this.propsNew[i]] = "我是数据";
          // eval("this.tableData2["+j+"].[this.propsNew["+i+"]]=0" );
        }
        this.tableData2.push(tempObj);
      }
    },
    handle(row, column, cell, event) {
      console.log(cell)
      if (cell.style.background == "yellow") {
        cell.style.background = "";
      } else {
        cell.style.background = "yellow";
      }
    }
  }
};
</script>
<style scoped lang="scss">
.el-input {
  margin-right: 14px;
  width: 180px;
  input {
    height: 32px;
  }
}
.el-table .active {
  background-color: yellow;
}
</style>
