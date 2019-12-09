<template>
    <div class="storeNoticeItem-wrapper">
        <div class="show-detail">
            <el-table :data="reTableData" border style="width: 100%">
                <el-table-column prop="date" label="日期" width="180">
                </el-table-column>
                <el-table-column prop="name" label="姓名" width="180">
                </el-table-column>
                <el-table-column prop="address" label="地址"> </el-table-column>
                <el-table-column prop="address" label="操作">
                    <template slot-scope="scope">
                        <el-button
                            size="mini"
                            type="danger"
                            @click="handleItemCancel(scope.$index, scope.row)"
                            >取消</el-button
                        >
                        <el-button
                            size="mini"
                            @click="handleItemSure(scope.$index, scope.row)"
                            >确定</el-button
                        >
                    </template>
                </el-table-column>
            </el-table>
            <el-upload
                :action="difficulty()"
                list-type="picture-card"
                :with-credentials="true"
                :on-success="succeed"
                :on-preview="handlePictureCardPreview"
                :on-remove="handleRemove"
                :on-exceed="beyond"
                :limit="5"
                name="adv_content"
                accept=".jpg,.jpeg,.png,.gif,.JPG,.JPEG,.GIF,.PNG,"
            >
                <i class="el-icon-plus"></i>
            </el-upload>
            <el-dialog :visible.sync="dialogVisible">
                <img width="100%" :src="dialogImageUrl" alt="" />
            </el-dialog>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            resStatus: "", //正确获取图片状态
            checkPic: [], //检查图片是否满足要求
            picArr: [], //接受多张图片
            figureCollection: [], //图片路径
            dialogImageUrl: "",
            dialogVisible: false
        };
    },
    props: {
        reTableData: Array,
        default: []
    },
    //生命周期 - 创建完成（访问当前this实例）
    created() {},
    //生命周期 - 挂载完成（访问DOM元素）
    mounted() {},
    methods: {
        handleItemCancel() {
            this.$emit("cancel");
        },
        //确定举报上传
        handleItemSure(index,data) {
            console.log(index,data);
        },
        //上传路径
        difficulty() {
            return this.$httpConfig.upLoadImage;
        },
        //上传成功，在数组中添加获取后的路径
        succeed(response, file, fileList) {
            //判断有没有上传图片
            this.resStatus = fileList;
            //存储删除的状态
            this.checkPic.push(Number(response.status));
            //将传入的图片放进数组
            this.picArr.push(response.data);
            this.figureCollection.push(file.response.data);
        },
        //取消图片上传
        handleRemove(file, fileList) {
            let index = this.figureCollection.indexOf(file.response.data);
            if (index > -1) {
                console.log("index值:" + index);
                this.figureCollection.splice(index, 1);
                this.picArr.splice(index, 1);
                this.checkPic.splice(index, 1);
            }
            //后台删除当前图片
            this.HTTP(
                this.$httpConfig.delPic,
                {
                    fileName: file.response.data
                },
                "post"
            )
                .then(e => {
                    console.log(e);
                })
                .catch(e => {
                    console.log(e);
                });
        },
        // 图片最大数之后
        beyond() {
            this.$message.warning("做多五张图片");
        },
        //图片预处理
        handlePictureCardPreview(file) {
            this.dialogImageUrl = file.url;
            this.dialogVisible = true;
        }
    },
    components: {}
};
</script>
<style lang="less">
.storeNoticeItem-wrapper {
    .el-upload__input {
        display: none;
    }
}
</style>
<style lang="less" scoped>
.storeNoticeItem-wrapper {
    position: fixed;
    top: 0;
    background: rgba(0, 0, 0, 0.3);
    height: 100%;
    width: 100%;
    z-index: 1111;
    .show-detail {
        position: fixed;
        height: 100%;
        width: 100%;
        background: #fff;
    }
}
</style>
