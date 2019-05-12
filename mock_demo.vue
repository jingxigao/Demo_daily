<template>
    <div>
        <el-button type="primary" @click="dialogAddSource = true"><i class="el-icon-plus"></i> 新增资源</el-button>
        <el-button type="primary" @click="dialogExportForm = true"><i class="fa fa-external-link"></i> 导出</el-button>
        <el-button type="primary" @click="dialogExportAllForm = true"  style="float:right"><i class="el-icon-tickets"></i> 导出总表</el-button>
        <el-input class="u-search" placeholder="请输入搜索内容" v-model="searchInput"></el-input>
        <div>
            <el-table :data="tables.slice((currentPage-1)*pagesize,currentPage*pagesize)" style="width:100%; margin-top:20px">
                <el-table-column label="序号" type="index" fixed width="50"></el-table-column>
                <el-table-column label="名称" fixed width="130px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.name }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="IP" width="150px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.ip }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="CPU核数（核）" width="120px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.cpu }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="内存（GB）" width="100px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.mem }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="计算费用（/月/天）" width="140px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.compute }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="SATA" width="60px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.sata }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="SAS" width="60px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.sas }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="SSD" width="60px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.ssd }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="存储费用（/月）" width="140px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.storage }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="业务系统" width="140px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.system }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="所属中心" width="140px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.center }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="所属部门" width="140px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.department }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="负责人" width="100px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.person }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="日期" width="160px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.time }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="生命周期信息" width="200px">
                    <template slot-scope="scope">
                        <span>{{ scope.row.remark }} - {{ scope.row.time }}</span>
                    </template>
                </el-table-column>
                <el-table-column label="操作" width="50px" fixed="right">
                    <template slot-scope="scope">
                      <el-dropdown @command="handleCommand">
                        <span class="el-dropdown-link">
                          <i class="el-icon-more" style="font-size:25px"></i>
                        </span>
                        <el-dropdown-menu slot="dropdown" style="width:100px">
                          <el-dropdown-item :command="`1-${scope.$index}`">变更资源</el-dropdown-item>
                          <el-dropdown-item :command="`2-${scope.$index}`">删除</el-dropdown-item>
                        </el-dropdown-menu>
                      </el-dropdown>
                    </template>
                </el-table-column>
            </el-table>
        </div>
        <div v-if="show">
            <el-pagination class="mt-20 text-center" :current-page="currentPage" :page-sizes="[10,20,50]"
            :page-size="5" layout="total, sizes, prev, pager, next, jumper" :total="tables.length" @current-change='handleChangCurrent' @size-change="handleSizeChange">
            </el-pagination>
        </div>
        <!-- 新增资源 -->
        <div>
            <el-dialog
                title="新增资源"
                :visible.sync="dialogAddSource"
                width="800px">
                <el-form :model="addInfo" style="margin-left:20px" label-width="100px">
                    <el-row>
                        <el-col :span="10">
                            <el-form-item label="名称：">
                                <el-input v-model="addInfo.name" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="IP：">
                                <el-input v-model="addInfo.ip" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="CPU：">
                                <el-input v-model="addInfo.cpu" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="内存：">
                                <el-input v-model="addInfo.mem" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="SATA：">
                                <el-input v-model="addInfo.sata" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="SAS：">
                                <el-input v-model="addInfo.sas" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="SSD：">
                                <el-input v-model="addInfo.ssd" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="10">
                            <el-form-item label="业务系统：">
                                <el-input v-model="addInfo.system" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="所属中心：">
                                <el-select v-model="addInfo.center" placeholder="请选择">
                                    <el-option
                                    v-for="item in center"
                                    :key="item.id"
                                    :label="item.label"
                                    :value="item.value">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item label="所属部门：">
                                <el-select v-model="addInfo.department" placeholder="请选择">
                                    <el-option
                                    v-for="item in addDepartment"
                                    :key="item.id"
                                    :label="item.label"
                                    :value="item.value">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item label="负责人：">
                                <el-input v-model="addInfo.person" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="时间：">
                                <div class="block">
                                    <el-date-picker
                                    v-model="addInfo.time"
                                    type="datetime"
                                    placeholder="选择日期时间">
                                    </el-date-picker>
                                </div>
                            </el-form-item>
                            <el-form-item label="操作信息：">
                                <el-input type="textarea" :rows="5" placeholder="请输入内容" v-model="addInfo.remark"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>
                </el-form>
                <span slot="footer" class="dialog-footer">
                    <el-button @click="dialogAddSource = false">取 消</el-button>
                    <el-button type="primary" @click="addSource">确 定</el-button>
                </span>
            </el-dialog>
        </div>
        <!-- 导出资源总表 -->
        <div>
            <el-dialog
                title="导出表格"
                :visible.sync="dialogExportAllForm"
                width="500px">
                <span>是否导出云资源汇总表？</span>
                <span slot="footer" class="dialog-footer">
                    <el-button @click="dialogExportAllForm = false">取 消</el-button>
                    <el-button type="primary" @click="exportForm">确 定</el-button>
                </span>
            </el-dialog>
        </div>
        <!-- 导出部门账单 -->
        <div>
            <el-dialog
                title="导出表格"
                :visible.sync="dialogExportForm"
                width="500px">
                <el-form :model="exportFormInfo" style="margin-left:20px" label-width="100px">
                    <el-form-item label="所属中心：">
                        <el-select v-model="exportFormInfo.center" placeholder="请选择">
                            <el-option
                            v-for="item in center"
                            :key="item.id"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item label="所属部门：">
                        <el-select v-model="exportFormInfo.department" placeholder="请选择">
                            <el-option
                            v-for="item in exportDepartment"
                            :key="item.id"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>
                    </el-form-item>
                </el-form>
                <span slot="footer" class="dialog-footer">
                    <el-button @click="dialogExportForm = false">取 消</el-button>
                    <el-button type="primary" @click="exportForm">确 定</el-button>
                </span>
            </el-dialog>
        </div>
        <!-- 变更资源 -->
        <div>
            <el-dialog
                title="变更资源"
                :visible.sync="dialogChangeSource"
                width="800px">
                <el-form :model="changeInfo" style="margin-left:20px" label-width="100px">
                    <el-row>
                        <el-col :span="10">
                            <el-form-item label="名称：">
                                <el-input v-model="changeInfo.name" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="IP：">
                                <el-input v-model="changeInfo.ip" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="CPU：">
                                <el-input v-model="changeInfo.cpu" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="内存：">
                                <el-input v-model="changeInfo.mem" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="SATA：">
                                <el-input v-model="changeInfo.sata" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="SAS：">
                                <el-input v-model="changeInfo.sas" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="SSD：">
                                <el-input v-model="changeInfo.ssd" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="10">
                            <el-form-item label="业务系统：">
                                <el-input v-model="changeInfo.system" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="所属中心：">
                                <el-select v-model="changeInfo.center" placeholder="请选择">
                                    <el-option
                                    v-for="item in center"
                                    :key="item.id"
                                    :label="item.label"
                                    :value="item.value">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item label="所属部门：">
                                <el-select v-model="changeInfo.department" placeholder="请选择">
                                    <el-option
                                    v-for="item in changeDepartment"
                                    :key="item.id"
                                    :label="item.label"
                                    :value="item.value">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item label="负责人：">
                                <el-input v-model="changeInfo.person" auto-complete="off" style="width:200px"></el-input>
                            </el-form-item>
                            <el-form-item label="时间：">
                                <div class="block">
                                    <el-date-picker
                                    v-model="changeInfo.time"
                                    type="datetime"
                                    placeholder="选择日期时间">
                                    </el-date-picker>
                                </div>
                            </el-form-item>
                            <el-form-item label="操作信息：">
                                <el-input type="textarea" :rows="5" placeholder="请输入内容" v-model="changeInfo.remark"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>
                </el-form>
                <span slot="footer" class="dialog-footer">
                    <el-button @click="dialogChangeSource = false">取 消</el-button>
                    <el-button type="primary" @click="changeSource">确 定</el-button>
                </span>
            </el-dialog>
        </div>
        <!-- 删除资源 -->
        <div>
            <el-dialog
                title="删除资源"
                :visible.sync="dialogDeleteSource"
                width="500px">
                <span>
                    <i class="fa fa-exclamation-circle"></i> 确定删除<span style="font-size:20px; color:red"> {{deleteName}} </span>计算资源吗？
                </span>
                <span slot="footer" class="dialog-footer">
                    <el-button @click="dialogDeleteSource = false">取 消</el-button>
                    <el-button type="primary" @click="deleteSource">确 定</el-button>
                </span>
            </el-dialog>
        </div>
    </div>
</template>
<script>
import ajax from "../../api/http/open.js";
import Vue from 'vue';
import axios from 'axios';
export default {
    data(){
        return{
            currentPage:1, //页码
            pagesize:10, //单页显示个数
            show:true, //展示页码
            searchInput:'', //搜索输入
            list:[],
            center:[
                {id:1, lable:'ITC', value:'ITC'},
                {id:2, lable:'CIO', value:'CIO'},
                {id:3, lable:'CFO', value:'CFO'},],
            addDepartment:[],
            changeDepartment:[],
            exportDepartment:[],
            dialogAddSource:false,
            addInfo:{
                name:'', 
                ip:'',
                cpu:'',
                mem:'',
                sata:'',
                sas:'',
                ssd:'',
                system:'',
                center:'',
                department:'',
                person:'',
                time:'',
                remark:''},
            dialogExportForm:false,
            exportFormInfo:{
                center:'',
                department:''},
            dialogExportAllForm:false,
            dialogChangeSource:false,
            changeInfo:{
                name:'',
                ip:'',
                cpu:'',
                mem:'',
                sata:'',
                sas:'',
                ssd:'',
                system:'',
                center:'',
                department:'',
                person:'',
                time:'',
                remark:''},
            deleteName:'',
            deleteId:'',
            dialogDeleteSource:false,
        }
    },
    watch:{
        addInfo:{
            handler(newval){
                if(newval.center == 'CIO'){
                    this.addDepartment = [{id: 1,label: '企划中心',value: '企划中心'}]
                }else if(newval.center == 'CFO'){
                    this.addDepartment = [{id:1,label:'预算中心',value:'预算中心'},{id:2,label:'资金中心',value:'资金中心'}]
                }else if(newval.center == 'ITC'){
                    this.addDepartment = [{id: 1,label: '云服务本部',value: '云服务本部'}]
                }
            },
            deep:true
        },
        changeInfo:{
            handler(newval){
                if(newval.center == 'CIO'){
                    this.changeDepartment = [{id: 1,label: '企划中心',value: '企划中心'}]
                }else if(newval.center == 'CFO'){
                    this.changeDepartment = [{id:1,label:'预算中心',value:'预算中心'},{id:2,label:'资金中心',value:'资金中心'}]
                }else if(newval.center == 'ITC'){
                    this.changeDepartment = [{id: 1,label: '云服务本部',value: '云服务本部'}]
                }
            },
            deep:true
        },
        exportFormInfo:{
            handler(newval){
                if(newval.center == 'CIO'){
                    this.exportDepartment = [{id: 1,label: '企划中心',value: '企划中心'}]
                }else if(newval.center == 'CFO'){
                    this.exportDepartment = [{id:1,label:'预算中心',value:'预算中心'},{id:2,label:'资金中心',value:'资金中心'}]
                }else if(newval.center == 'ITC'){
                    this.exportDepartment = [{id: 1,label: '云服务本部',value: '云服务本部'}]
                }
            },
            deep:true
        }
    },
    computed:{
        // 模糊搜索
        tables () {
            const searchInput = this.searchInput;
            this.currentPage = 1;
            if (searchInput) {
                // filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。
                // 注意： filter() 不会对空数组进行检测。
                // 注意： filter() 不会改变原始数组。
                return this.list.filter(data => {
                // some() 方法用于检测数组中的元素是否满足指定条件;
                // some() 方法会依次执行数组的每个元素：
                // 如果有一个元素满足条件，则表达式返回true , 剩余的元素不会再执行检测;
                // 如果没有满足条件的元素，则返回false。
                // 注意： some() 不会对空数组进行检测。
                // 注意： some() 不会改变原始数组。
                return Object.keys(data).some(key => {
                    // indexOf() 返回某个指定的字符在某个字符串中首次出现的位置，如果没有找到就返回-1；
                    // 该方法对大小写敏感！所以之前需要toLowerCase()方法将所有查询到内容变为小写。
                    return String(data[key]).toLowerCase().indexOf(searchInput) > -1
                })
                })
            }
            return this.list
        }
    },
    created(){
        this.getComputeList()
    },
    methods:{
        // 获取计算资源列表
        getComputeList(){
            ajax.getComputeList().then((response) => {
                this.list = response.data
            });
        },
        //改变页码
        handleChangCurrent(page){
            this.currentPage = page;
        },
        //更改单页显示数量
        handleSizeChange(pagesize){
            this.pagesize = pagesize;
        },
        // 添加资源
        addSource(){
            this.dialogAddSource = false;
            ajax.postAddCompute(this.addInfo).then((response) => {
                this.list = response.data;
            });
        },
        // 导出表格
        exportForm(){
            this.dialogExportForm = false;
            this.dialogExportAllForm = false;
        },
        // 下载文件
        downloadfile(path) {
        axios({
            method:"get",
            url:'',
            data:{},
            headers:{
                "authorization":"Bearer " + localStorage.accessToken
            },
            responseType:'blob',
        })
        .then(response => {
            let csvData = new Blob([response.data],{ type: 'text/csv' })
            if (window.navigator && window.navigator.msSaveOrOpenBlob) {
            window.navigator.msSaveOrOpenBlob(csvData, path);
            }else{
            let url = window.URL.createObjectURL(new Blob([response.data]));
            var a = document.createElement("a");
            document.body.appendChild(a);
            a.href = url;
            a.download = path;
            a.click();
            window.URL.revokeObjectURL(url);
            }
            // let url = window.URL.createObjectURL(response.data);
        }).catch(() => {
            this.$message({
                type: "error",
                message: "下载失败！"
            });
        })
        },
        //变更资源
        changeSource(){
            this.dialogChangeSource = false;
            Vue.set(this.changeInfo, 'id', this.deleteId);
            ajax.postModifyCompute(this.changeInfo).then((response) => {
                this.list = response.data;
            });
        },
        //删除资源
        deleteSource(){
            this.dialogDeleteSource = false;
            ajax.postDeleteCompute(this.deleteId).then((response) => {
                this.list = response.data
            });
        },
        // 操作
        handleCommand(command){
            const commandArray = command.split("-");
            if(commandArray[0] == 1){
                this.dialogChangeSource = true;
                this.changeInfo.name = this.tables[commandArray[1]].name;
                this.changeInfo.ip = this.tables[commandArray[1]].ip;
                this.changeInfo.cpu = this.tables[commandArray[1]].cpu;
                this.changeInfo.mem = this.tables[commandArray[1]].mem;
                this.changeInfo.sata = this.tables[commandArray[1]].sata;
                this.changeInfo.sas = this.tables[commandArray[1]].sas;
                this.changeInfo.ssd = this.tables[commandArray[1]].ssd;
                this.changeInfo.system = this.tables[commandArray[1]].system;
                this.changeInfo.center = this.tables[commandArray[1]].center;
                this.changeInfo.department = this.tables[commandArray[1]].department;
                this.changeInfo.person = this.tables[commandArray[1]].person;
                this.changeInfo.time = '';
                this.changeInfo.remark = '';
                this.deleteId = commandArray[1];
            }else if(commandArray[0] == 2){
                this.deleteName = this.tables[commandArray[1]].name;
                this.deleteId = commandArray[1];
                this.dialogDeleteSource = true;
            }
        },
    }
}
</script>
<style lang="scss" scoped>
    .u-search{
        width: 200px;
        margin-left: 20px
    }
</style>
