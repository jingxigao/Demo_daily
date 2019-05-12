    methods:{
        // 获取资源列表
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
        }
    }
}
</script>
