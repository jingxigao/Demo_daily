<template>
  <div>
    <el-row>
      <el-input placeholder="请输入行数" v-model="rowNum"></el-input>
      <el-input placeholder="请输入列数" v-model="columnNum"></el-input>
      <el-button type="primary" size="small" @click="generateTable">生成表格</el-button>
    </el-row>
    <div class="list-content">
      <el-table :data="tableData2" @cell-click="handle">
        <el-table-column
          v-for="(item,index) in propsNew"
          width="100"
          :key="index"
          align="center"
          :prop="item"
        ></el-table-column>
      </el-table>
    </div>
    <el-button type="primary" size="small" @click="judge">点击判断</el-button>
    <el-dialog title="该图形是：" :visible.sync="dialogVisible" width="30%">
      <span>{{isRegular}}</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
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
      pointVec: [],
      isRegular: "",
      dialogVisible: false
    };
  },
  methods: {
    generateTable() {
      this.propsNew = [];
      this.tableData2 = [];
      this.pointVec = [];
      for (var i = 0; i < this.columnNum; i++) {
        var colY = "Column" + (i + 1);
        this.propsNew.push(colY);
      }
      for (var j = 0; j < this.rowNum; j++) {
        var tempObj = {};
        for (var i = 0; i < this.columnNum; i++) {
          tempObj[this.propsNew[i]] = `${j+1},${i+1}`;
        }
        this.tableData2.push(tempObj);
      }
      var tableList = document.getElementsByTagName("td");
      for (let i = 0; i < tableList.length; i++) {
        tableList[i].style.background = "";
      }
    },
    handle(row, column, cell, event) {
      if (cell.style.background == "yellow") {
        cell.style.background = "";
      } else {
        cell.style.background = "yellow";
      }
    },
    judge() {
      this.pointVec=[]
      this.dialogVisible = true;
      var tableList = document.getElementsByTagName("td");
      for (let i = 0; i < tableList.length; i++) {
        if (tableList[i].style.background == "yellow") {
          var arr = tableList[i].firstChild.innerHTML.split(",");
          var newPoint = arr.map(function(item) {
            return +item;
          });
          this.pointVec.push(newPoint);
        }
      }
      if(this.rowNum==0 || this.columnNum==0 ){
          this.isRegular="请认真输入表格行数及列数！"
          return
      }
      if(this.pointVec.length==0){
          this.isRegular="请选择表格内容！呆子"
          return
      }
      var minX = this.pointVec[0][0];
      var maxX = this.pointVec[0][0];
      var minY = this.pointVec[0][1];
      var maxY = this.pointVec[0][1];

      for (let i = 0; i < this.pointVec.length; i++) {
        if (this.pointVec[i][0] > maxX) {
          maxX = this.pointVec[i][0];  
        }
        if (this.pointVec[i][0] < minX) {
          minX = this.pointVec[i][0];
        }
        if (this.pointVec[i][1] > maxY) {
          maxY = this.pointVec[i][1]; 
        }
        if (this.pointVec[i][1] < minY) {
          minY = this.pointVec[i][1];
        }
      }
      var deltX = maxX - minX + 1;
      var deltY = maxY - minY + 1;
      var sizeXY = deltX * deltY;
      if (this.pointVec.length == sizeXY) {
        this.isRegular="规则的"
      }else{
        this.isRegular="不规则的"
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
</style>
