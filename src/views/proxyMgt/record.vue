<template>
    <div class="main">
        <el-table
        :data="tableData"
        style="width: 100%">
            <el-table-column
                align="center"
                label="提现账号"
                prop="userBack.receiveAccount"
                width="200">
            </el-table-column>
           <el-table-column
                align="center"
                label="提现金额"
                prop="outAmout"
                width="400">
            </el-table-column>
            <el-table-column
                align="center"
                label="提现时间"
                prop="createDate"
                width="400">
            </el-table-column>
        </el-table>
        <el-pagination
            class="pagination"
            background
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="currentPage"
            :page-sizes="[10, 20, 40, 100]"
            :page-size="10"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total">
        </el-pagination>
    </div>
</template>

<script>
    import request from '@/utils/request'
    export default {
        data(){
            return{
                tableData: [],
                total:0,
                currentPage:1,
                pageNo:1,
                pageSize:10,
                userId:''
            }
        },
        mounted(){
            this.userId=this.$route.query.userId
            this.getList()
        },
        methods:{
             getList(){
                request({
                    url: '/system/cashOut/list',
                    method: 'get',
                    params:{
                        pageNo:this.pageNo,
                        pageSize:this.pageSize,
                        userId:this.userId,
                    }
                }).then((res)=>{
                    console.log(res)
                    this.tableData=res.data
                    this.total=res.count
                })
            },
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
                this.pageSize=val
                this.pageNo=1
                this.getList()
            },
            handleCurrentChange(val) {
                console.log(`当前页: ${val}`);
                this.pageNo=val
                this.getList()
            },
           
        }
    }
</script>
<style lang="scss" scoped>
     .main{
        padding:30px;
        .pagination{
            float:right;
            margin-top:30px;
        }

    }
</style>