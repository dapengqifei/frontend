<template>
    <el-table
            :data="tableData"
            style="width: 100%">
        <el-table-column
                label="书名"
                width="180">
            <template slot-scope="scope">
                <i class="el-icon-time"></i>
                <span style="margin-left: 10px">{{ scope.row.title }}</span>
            </template>
        </el-table-column>
        <el-table-column
                label="作者"
                width="180">
            <template slot-scope="scope">
                <el-popover trigger="hover" placement="top">
                    <p>姓名: {{ scope.row.title }}</p>
                    <p>住址: {{ scope.row.content }}</p>
                    <div slot="reference" class="name-wrapper">
                        <el-tag size="medium">{{ scope.row.content }}</el-tag>
                    </div>
                </el-popover>
            </template>
        </el-table-column>
        <el-table-column label="操作">
            <template slot-scope="scope">
                <el-button
                        size="mini"
                        @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                <el-button
                        size="mini"
                        type="danger"
                        @click="handleDelete(scope.$index, scope.row)">删除</el-button>
            </template>
        </el-table-column>
    </el-table>
</template>

<script>
    export default {
        data() {
            return {
                tableData: [{
                    date: '2016-05-02',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1518 弄'
                }, {
                    date: '2016-05-04',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1517 弄'
                }, {
                    date: '2016-05-01',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1519 弄'
                }, {
                    date: '2016-05-03',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1516 弄'
                }],
                base_url: '/api/blog/',
                blogs:null,
                url:'',
                title:'',
                content:'',
                status:'',
                txt:'22'
            }
        },
        methods: {
            handleEdit(index, row) {
                console.log(index, row);
            },
            handleDelete(index, row) {
                console.log(index, row);
            },
            getAll(){
                this.$axios.get(this.base_url)
                    .then(res=>{
                        console.log(res)
                        this.tableData = res.data;
                        this.url = '';
                        this.title = ''
                        this.content = ''

                    });

            },
            demo(){
                this.$axios.get(this.base_url)
                    .then(res=>{
                        this.blogs = res.data;
                        this.status = res.status;
                    });

            },
            saveBlog(){
                // 两种情况，一种是新建博客，一种是修改博客,
                // 通过 url是否为空来判断
                if(this.url == ''){
                    // 新增
                    this.$axios.post(this.base_url,{title:this.title, content:this.content,txt:this.txt})
                        .then(()=>{
                            this.getAll();
                        })
                }else{
                    //修改 因为要修改的url已经配置好了
                    this.$axios.put(this.url, {title:this.title, content:this.content} )
                        .then(()=>{
                            this.getAll();
                        })
                }

            },
            editBlog(blog){
                this.url = blog.url;
                this.title = blog.title;
                this.content = blog.content;
            },
            deleteBlog(blog) {
                this.axios.delete(blog.url)
                    .then(() => {
                        this.getAll();
                    });
            }
        },
        created(){
            console.log(this)
            this.getAll();
        }
    }
</script>>
