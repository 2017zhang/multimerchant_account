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
                <template slot-scope="scope">{{ scope.row.date }}</template>
            </el-table-column>
            <el-table-column prop="name" label="商品图" width="120">
            </el-table-column>
            <el-table-column prop="name" label="商品名称" width="120">
            </el-table-column>
            <el-table-column prop="name" label="本店价格(元)" width="120">
            </el-table-column>
            <el-table-column prop="name" label="库存" width="120">
            </el-table-column>
            <el-table-column prop="name" label="状态" width="120">
            </el-table-column>
            <el-table-column prop="name" label="审核状态" width="120">
            </el-table-column>
            <el-table-column prop="name" label="发布时间" width="120">
            </el-table-column>
            <el-table-column label="操作">
                <template slot-scope="scope">
                    <el-button
                        size="mini"
                        @click="handleEdit(scope.$index, scope.row)"
                        >编辑</el-button
                    >
                    <el-button
                        size="mini"
                        type="danger"
                        @click="handleDelete(scope.$index, scope.row)"
                        >删除</el-button
                    >
                </template>
            </el-table-column>
        </el-table>
        <!-- 用于显示举报商品界面 -->
        <store-item
            v-if="showNoticeItem"
            :reTableData="reTableData"
            @cancel="cancel"
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
            reTableData: []
        };
    },
    //生命周期 - 创建完成（访问当前this实例）
    created() {},
    //生命周期 - 挂载完成（访问DOM元素）
    mounted() {},
    methods: {
        handleSelectionChange(val) {
            this.multipleSelection = val;
            console.log(val, 111);
        },
        handleEdit(index, row) {
            console.log(index, row);
            this.reTableData=this.tableData.slice(index,index+1);
            this.showNoticeItem = true;
        },
        handleDelete(index, row) {
            console.log(index, row);
        },
        //详情隐藏显示
        cancel() {
            this.showNoticeItem = false;
        },
    },
    components: {
        StoreItem
    }
};
</script>
<style lang="less">
.storeNotice-wrapper {
}
</style>
<style lang="less" scoped>
.storeNotice-wrapper {
}
</style>
