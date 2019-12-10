<template>
    <div class="storeNotice-wrapper">
        <div class="header">
            <h6>举报违规-列表:</h6>
            <div class="wrapper">
                <div class="notice">温馨提示</div>
                <ul>
                    <li>1<span>请填写证明内容</span></li>
                    <li>2<span>请上传相关证明</span></li>
                    <li>3<span>提交后,工作人员会在3-5个工作日结算</span></li>
                </ul>
            </div>
            <div class="info">
                <h5>举报类型:</h5>
                <el-select v-model="selectValue">
                    <el-option
                        v-for="item in options"
                        :key="item.value"
                        :label="item.label"
                        :value="item.value"
                    >
                    </el-option>
                </el-select>
                <el-button>搜索</el-button>
            </div>
        </div>
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
            <el-table-column prop="name" label="举报时间" width="180">
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
            :reportDetailData="reportDetailData"
            @goBackTop="goBackTop"
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
            reportDetailData: {},
            selectValue:'',
            options: [
                {
                    value: "选项1",
                    label: "黄金糕"
                }
            ]
        };
    },
    //生命周期 - 创建完成（访问当前this实例）
    created() {
        let arr=[{
            name:'jj'
        },{
            name:'kk'
        }]
        arr.forEach(el=>{
            this.$set(el,'id',1)
        })
        console.log(arr,'arr');
    },
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
        goBackTop() {
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
    .el-input__inner {
        margin-left: 15px;
    }
    .el-button {
        margin-left: 30px;
    }
}
</style>
<style lang="less" scoped>
.storeNotice-wrapper {
    .header {
        h6 {
            font-size: 16px;
            border-bottom: 1px solid #e8e8e8;
            color: #333;
            padding: 10px 0;
        }
        .wrapper {
            background: rgba(79, 192, 232, 0.11);
            border-left: 2px solid #0ba4da;
            margin: 15px 0;
            .notice {
                font-size: 16px;
                color: #0ba4da;
                padding-left: 8px;
                padding-top: 20px;
            }
            ul {
                display: flex;
                flex-direction: column;
                padding: 20px;
                li {
                    color: #0ba4da;
                    font-size: 13px;
                    span {
                        color: #0ba4da;
                        font-size: 13px;
                        margin: 10px 5px;
                    }
                }
            }
        }
        .info {
            display: flex;
            align-items: center;
            background: #efefef;
            padding: 5px;
            margin: 10px 0;
        }
    }
}
</style>
