<template>
    <div class="serviceMessage-wrapper">
        <div class="intrduce">
            <div class="title">
                说明:
            </div>
            <ul>
                <li>1.管理员可以查看全部信息</li>
                <li>
                    2.只有管理员可以删除信息,删除后其他账户该条信息也会被删除
                </li>
            </ul>
        </div>
        <el-table
            ref="multipleTable"
            :data="serviceTableData"
            tooltip-effect="dark"
            style="width: 100%"
            @selection-change="handleSelectionChange"
        >
            <el-table-column type="selection" width="55"> </el-table-column>
            <el-table-column width="250">
                <template slot="header" slot-scope="scope">
                    <el-button icon="el-icon-s-flag" size="mini"
                        >标记为已读</el-button
                    >
                    <el-button icon="el-icon-delete" size="mini"
                        >删除</el-button
                    >
                </template>
            </el-table-column>
            <el-table-column label="消息内容" width="260">
                <template slot-scope="scope">{{
                    scope.row.sm_content
                }}</template>
            </el-table-column>
            <el-table-column label="发送时间" width="260">
                <template slot-scope="scope">{{
                    scope.row.sm_addtime | formatDate
                }}</template>
            </el-table-column>
            <el-table-column label="操作">
                <template slot-scope="scope">
                    <el-button
                        size="mini"
                        @click="handleEdit(scope.$index, scope.row)"
                        >查看</el-button
                    >
                </template>
            </el-table-column>
        </el-table>
        <el-pagination
            @current-change="handleCurrentChange"
            :current-page="currentPage"
            @size-change="handleSizeChange"
            :page-size="pagesize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total"
        >
        </el-pagination>
    </div>
</template>

<script>
export default {
    data() {
        return {
            serviceTableData: [],
            multipleSelection: [],
            currentPage: 1,
            total: 0,
            pagesize: 10,
            timer: ""
        };
    },
    //生命周期 - 创建完成（访问当前this实例）
    created() {
        this.getServiceInfo();
        this.getServiceData();
        this.timer = setInterval(this.getServiceData, 600000);
    },
    beforeDestroy() {
        clearInterval(this.timer);
    },
    //生命周期 - 挂载完成（访问DOM元素）
    mounted() {},
    methods: {
        getServiceData() {
            this.$HTTP(this.$httpConfig.serviceMsgCount, {})
                .then(res => {
                    this.$store.state.messageCount=res.data.data;
                    console.log(res.data.data,221133);
                })
                .catch(err => {
                    console.error(err);
                });
        },
        getServiceInfo() {
            this.$HTTP(this.$httpConfig.serviceListInfo, {
                page: this.currentPage
            })
                .then(res => {
                    this.serviceTableData = res.data.data.data;
                    this.total = Number(res.data.data.count);
                    console.log(res.data.data);
                })
                .catch(err => {
                    console.error(err);
                });
        },
        handleSelectionChange(val) {
            this.multipleSelection = val;
        },
        handleCurrentChange(currentPage) {
            this.currentPage = currentPage;
            this.getServiceInfo();
        },
        // 初始页currentPage、初始每页数据数pagesize和数据data
        handleSizeChange(size) {
            this.pagesize = size;
            console.log(this.pagesize); //每页下拉显示数据
        }
    },
    components: {}
};
</script>
<style lang="less">
.serviceMessage-wrapper {
    .el-pagination {
        text-align: center;
        margin-top: 10px;
    }
    .el-pagination__sizes {
        display: none !important;
    }
}
</style>
<style lang="less" scoped>
.serviceMessage-wrapper {
    .intrduce {
        background: rgba(79, 192, 232, 0.11);
        .title {
            color: #0ba4da;
            font-size: 16px;
        }
        ul {
            li {
                margin: 5px 0;
                color: #0ba4da;
                font-size: 13px;
            }
        }
    }
}
</style>
