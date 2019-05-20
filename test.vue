<template>
  <div>
    <!-- <div v-if="flag">
      <div>
        <span class="titlespan">方案名称：</span>
        <el-input v-model="item.title1" placeholder="请方案名称" size="small"></el-input>
        <span>{{item.data}}</span>
      </div>
    </div>-->
    <!-- <div v-if="!flag">
      <div>
        <span class="titlespan">方案名称2：</span>
        <el-input v-model="item.title1" placeholder="请方案名称" size="small"></el-input>
        <el-input v-model="item.data" placeholder="data部分" size="small"></el-input>
      </div>
    </div>-->
    <!-- <el-button type="text" @click="flag=!flag">切换v-if</el-button> -->
    <table border="1" cellpadding="0" cellspacing="0" width="200" style="float:left;">
      <thead>
        <tr style="background-color: gray;">
          <th>Month</th>
          <th>Savings</th>
          <!-- <th v-for="(item,index) in tab" :key="index">item.title</th> -->
        </tr>
      </thead>

      <!-- <tfoot style="background-color: black;color: white;">
        <tr>
          <td>Sum</td>
          <td>$180</td>
        </tr>
      </tfoot>-->
      <tbody>
        <tr v-for="(item,index) in tab" :key="index">
          <td>{{item.title}}</td>
          <td>{{item.content}}</td>
        </tr>
      </tbody>
    </table>
    <div class="data">
      <el-input v-model="tab[0].title" placeholder="title" size="small"></el-input>
      <el-input v-model="tab[0].content" placeholder="content" size="small"></el-input>
      <el-input v-model="tab[1].title" placeholder="title" size="small"></el-input>
      <el-input v-model="tab[1].content" placeholder="content" size="small"></el-input>
      <el-input v-model="tab[2].title" placeholder="title" size="small"></el-input>
      <el-input v-model="tab[2].content" placeholder="content" size="small"></el-input>
    </div>
    <!-- <el-input v-model="table.t1" placeholder="请方案名称" size="small"></el-input>
    <el-input v-model="table.t2" placeholder="请方案名称" size="small"></el-input>
    <el-input v-model="table.t3" placeholder="请方案名称" size="small"></el-input>-->
  </div>
</template>
<script>
export default {
  name: "test",
  data() {
    return {
      flag: 1,
      item: {
        title1: "我是title",
        data: ""
      },
      table: {
        t1: "",
        t2: "",
        t3: ""
      },
      tab: [
        {
          title: "",
          content: ""
        },
        {
          title: "",
          content: ""
        },
        {
          title: "",
          content: ""
        }
      ]
    };
  }
};
</script>
<style lang="scss" scoped>
.el-input {
  display: inline-block;
  width: 200px;
}
.data {
  overflow: hidden;
}
</style>
