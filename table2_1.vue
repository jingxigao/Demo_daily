<template>
  <div id="app">
    <el-form :inline="true" :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
      <el-form-item label="行数" prop="row" required>
        <el-input v-model="ruleForm.row" placeholder="请输入行数"></el-input>
      </el-form-item>
    <el-form-item label="列数" prop="col" required>
      <el-input v-model="ruleForm.col" placeholder="请输入列数"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="createTable('ruleForm')">创建表格</el-button>
    </el-form-item>
    </el-form>

    <el-table
    v-show="showTable"
    ref="table"
    :data="tableData"
    highlight-current-row
    style="width: 100%"
    @cell-click="handleClick">
    <el-table-column
      v-for="item in colList"
      width="100"
      :key="item"
      align="center"
      :prop="item"
      >
      <template slot-scope="scope">
        <el-button type="text" size="small">{{scope.row[item]}}</el-button>
      </template>
    </el-table-column>
  </el-table>
    <el-button style="font-size: 20px; margin-top: 10px;" round type="info" v-show="showTable"  @click="check">检测</el-button>

    <el-dialog
      title="该图形是："
      :visible.sync="dialogVisible"
      width="30%">
      <span>{{isRegular}}</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script type="text/javascript">
import { truncate } from 'fs';
import { connect } from 'net';
export default {
  name: 'App',
  data () {
    return {
      ruleForm: {
        row: '',
        col: ''
      },
      rules: {
        row: [
          { required: true, message: '请输入行数', trigger: 'blur' },
          { min: 1, message: '行数至少大于1', trigger: 'blur' }
        ],
        col: [
          { required: true, message: '请输入列数', trigger: 'blur' },
          { min: 1, message: '列数至少大于1', trigger: 'blur' }
        ]
      },
      showTable: false,
      tableData: [],
      colList: [],
      isRegular: "",
      dialogVisible: false,
      chosenList: []
    }
  },
  methods: {
    createTable (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.showTable = true
          var arr = []
          for (let i = 0; i < this.ruleForm.row; i++) {
            let oneRow = {}
            for (let j = 0; j < this.ruleForm.col; j++) {
              oneRow[String(j)] = `${i},${j}`
            }
            arr.push(oneRow)
            console.log(arr)
          }
        }
        this.tableData = arr
        var colList = []
        for (let j = 0; j < this.ruleForm.col; j++) {
          colList.push(String(j))
        }
        this.colList = colList
      })
      var tableList = document.getElementsByClassName('el-button--text')
      for (let i = 0; i < tableList.length; i++) {
        if (this.hasClass(tableList[i], 'active')) {
          this.removeClass(tableList[i], 'active')
        }
      }
      this.chosenList = []
    },
    hasClass (ele, cls) {
      return ele.className.match(new RegExp('(\\s|^)' + cls + '(\\s|$)'))
    },
    // 为指定的dom元素添加样式
    addClass (ele, cls) {
      if (!this.hasClass(ele, cls)) ele.className += ' ' + cls
    },
    // 删除指定dom元素的样式
    removeClass (ele, cls) {
      if (this.hasClass(ele, cls)) {
        var reg = new RegExp('(\\s|^)' + cls + '(\\s|$)')
        ele.className = ele.className.replace(reg, ' ')
      }
    },
    handleClick (row, column, cell, event) {
      if (this.hasClass(cell.getElementsByTagName('button')[0], 'active')) {
        this.removeClass(cell.getElementsByTagName('button')[0], 'active')
        var deleteIndex = this.chosenList.indexOf(cell.getElementsByTagName('span')["0"].innerText)
        this.chosenList.splice(deleteIndex, 1)
        return
      }
      this.addClass(cell.getElementsByTagName('button')[0], 'active')
      this.chosenList.push(cell.getElementsByTagName('span')["0"].innerText)
    },
    check() {
      this.dialogVisible = true
      let chosenListNum = []
      this.chosenList.map(m => {
          var item = [Number(m.split(',')[0]), Number(m.split(',')[1])]
          chosenListNum.push(item);
        })
        console.log(this.chosenList)
        console.log(chosenListNum)
      chosenListNum = this.orderArrayByXYCoordinates(chosenListNum)
      console.log(chosenListNum)
      if(this.chosenList.length <= 1) {
        this.isRegular = "规则的"
        return
      } else{
        let supposedList = this.formList(chosenListNum[0], chosenListNum[chosenListNum.length-1])
        this.isRegular = chosenListNum.toString() === supposedList.toString()? "规则的":"不规则的"
        return
      }
    },
    orderArrayByXYCoordinates(xyArray) {
        var i, j, t,
        n = xyArray.length;
        for(i=1; i<n; i++){
          for(j=0; j<n-i; j++){
            if(xyArray[j][0] == xyArray[j+1][0] && xyArray[j][1] > xyArray[j+1][1]){
              t = xyArray[j]
              xyArray[j]=xyArray[j+1]
              xyArray[j+1]= t
            }else if(xyArray[j][0] > xyArray[j+1][0]){
              t = xyArray[j];
              xyArray[j]=xyArray[j+1];
              xyArray[j+1]= t;
            }
          }
        }
        return xyArray;
    },
    formList(point1, point2) {
      let supposedList = []
      let item = []
      for (let i=point1[0]; i<=point2[0]; i++) {
        for (let j=point1[1]; j<=point2[1]; j++){
          item = [i,j]
          supposedList.push(item)
        }
      }
      return supposedList
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.el-table{
  margin-left: 50px;
}
.el-table .active{
  padding-left: 30px;
  padding-right: 30px;
  background-color:blue;
  color: white;
}
.el-table__header-wrapper th{
  color: blue;
  font-size: 30px;
}
</style>
