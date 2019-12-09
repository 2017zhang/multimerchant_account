<template>
    <div class="storeNotice-wrapper">
        <el-table
            ref="multipleTable"
            :data="tableData"
            tooltip-effect="dark"
            style="width: 100%"
            @selection-change="handleSelectionChange"
        >
            <el-table-column type="selection" width="55"> </el-table-column>
            <el-table-column label="编号" width="120">
                <template slot-scope="scope">{{ scope.row.id }}</template>
            </el-table-column>
            <el-table-column prop="title" label="商品名称" width="180">
            </el-table-column>
            <el-table-column prop="name" label="审核状态" width="120">
                <template slot-scope="scope">
                    <span v-if="scope.row.status == 0">待审核</span>
                    <span v-else>审核成功</span>
                </template>
            </el-table-column>
            <el-table-column prop="name" label="发布时间" width="180">
                <template slot-scope="scope">
                    <span>{{ scope.row.create_time | formatDate }}</span>
                </template>
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
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="currentPage"
            :page-size="pagesize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total"
        >
        </el-pagination>
        <!-- 用于显示举报商品界面 -->
        <store-item
            v-if="showNoticeItem"
            @cancel="cancel"
            :reportDetailData='reportDetailData'
            @goBackTop='goBackTop'
        ></store-item>
    </div>
</template>

<script>
import StoreItem from "./child/storeNoticeItem";
export default {
    data() {
        return {
            tableData: [
                {
                    date: "2016-05-02",
                    name: "王小虎",
                    address: "上海市普陀区金沙江路 1518 弄"
                },
                {
                    date: "2016-05-04",
                    name: "王小虎",
                    address: "上海市普陀区金沙江路 1517 弄"
                },
                {
                    date: "2016-05-01",
                    name: "王小虎",
                    address: "上海市普陀区金沙江路 1519 弄"
                },
                {
                    date: "2016-05-03",
                    name: "王小虎",
                    address: "上海市普陀区金沙江路 1516 弄"
                }
            ],
            multipleSelection: [],
            showNoticeItem: false,
            reTableData: [],
            currentPage: 1, //初始页
            pagesize: 10, //每页的数据
            total: 0, //总的分页条目
            reportDetailData:{}
        };
    },
    //生命周期 - 创建完成（访问当前this实例）
    created() {},
    //生命周期 - 挂载完成（访问DOM元素）
    mounted() {
        this.getReportList(this.currentPage);
    },
    methods: {
        //获取列表数据
        getReportList(currentPage) {
            this.$HTTP(
                this.$httpConfig.reportList,
                {
                    page: currentPage
                },
                "get"
            )
                .then(res => {
                    this.tableData = res.data.data.list;
                    this.total = Number(res.data.data.count);
                })
                .catch(err => {
                    console.error(err);
                });
        },
        // 初始页currentPage、初始每页数据数pagesize和数据data
        handleSizeChange(size) {
            this.pagesize = size;
            console.log(this.pagesize); //每页下拉显示数据
        },
        handleCurrentChange(currentPage) {
            this.currentPage = currentPage;
            this.getReportList(currentPage);
            console.log(this.currentPage); //点击第几页
        },
        handleSelectionChange(val) {
            this.multipleSelection = val;
            console.log(val, 111);
        },
        handleEdit(index, row) {
            this.showNoticeItem = true;
            //获取列表详情
            this.getDetailInfo(row.id);
        },
        getDetailInfo(id) {
            this.$HTTP(
                this.$httpConfig.reportDetail,
                {
                    id: id
                },
                "post"
            )
                .then(res => {
                    this.reportDetailData = res.data.data;
                    console.log(res.data.data, 444);
                })
                .catch(err => {
                    console.error(err);
                });
        },

        //返回上一级
        goBackTop(){
            this.showNoticeItem = false;
        },
        //详情隐藏显示
        cancel() {
            this.showNoticeItem = false;
        }
    },
    components: {
        StoreItem
    }
};
</script>
<style lang="less">
.storeNotice-wrapper {
    .el-pagination {
        text-align: center;
        margin: 20px 0;
    }
}
</style>
<style lang="less" scoped>
.storeNotice-wrapper {
}
</style>
