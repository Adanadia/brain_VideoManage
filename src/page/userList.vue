<template>
    <div class="fillcontain">
        <head-top></head-top>
        <div class="table_container">
            <el-table
                :data="tableData"
                highlight-current-row
                style="width: 100%">
                <el-table-column
                  type="index">
                </el-table-column>
                <el-table-column
                  property="name"
                  label="用户名">
                </el-table-column>
                <el-table-column
                  label="用户头像">
                    <template slot-scope="scope">
                        <img :src="tableData[scope.$index].avathor" width="40" height="40" class="head_pic"/>
                    </template>
                </el-table-column>
                <el-table-column
                    property="sid"
                    label="学号"
                    width="">
                </el-table-column>
                <el-table-column
                    property="desc"
                    label="描述">
                </el-table-column>
                <el-table-column
                    property="age"
                    label="年龄">
                </el-table-column>
                <el-table-column
                    property="sex"
                    label="性别">
                </el-table-column>
                <el-table-column
                    property="school"
                    label="学校">
                </el-table-column>
                <el-table-column
                    label="操作">
                    <template slot-scope="scope">
                        <el-button
                            size="mini"
                            type="danger"
                            @click="handleDelete(scope.$index, scope.row, tableData)">删除</el-button>
                    </template>
                </el-table-column>
            </el-table>
            <div class="Pagination" style="text-align: left;margin-top: 10px;">
                <el-pagination
                  @size-change="handleSizeChange"
                  @current-change="handleCurrentChange"
                  :current-page="currentPage"
                  :page-size="20"
                  layout="total, prev, pager, next"
                  :total="count">
                </el-pagination>
            </div>
        </div>
    </div>
</template>

<script>
    import headTop from '../components/headTop'
    // import {getUserList} from '@/api/getData'
    import {GET, POST} from '../request/http'
    export default {
        data(){
            return {
                tableData: [],
                currentRow: null,
                offset: 0,
                limit: 20,
                count: 0,
                currentPage: 1,
            }
        },
    	components: {
    		headTop,
    	},
        created(){
            this.getUsers();
        },
        methods: {
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                this.currentPage = val;
                this.offset = (val - 1)*this.limit;
                this.getUsers()
            },
            async getUsers(){
                const Users = await GET('/admin/userList');
                // console.log(Users.data.userInfo)
                this.tableData = Users.data.userInfo
                // console.log(this.tableData[0].avathor)
                this.count  = this.tableData.length
                this.currentPage = 1
            },
            async handleDelete(index, row, tableData) {
                console.log(index, row);
                let result = await POST('/admin/deleteUser',{sid: row.sid})
                if (result.status === 200){
                    this.$message({
                        message: '删除成功',
                        type: 'success'
                    });
                    tableData.splice(index, 1);
                    this.count --
                }
            }
        },
    }
</script>

<style lang="less">
	@import '../style/mixin';
    .table_container{
        padding: 20px;
    }
</style>
