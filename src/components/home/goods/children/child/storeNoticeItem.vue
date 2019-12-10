<template>
    <div class="storeNoticeItem-wrapper">
        <div class="show-detail">
            <div class="container">
                <h5>举报违规-列表:</h5>
                <div class="wrapper">
                    <div class="notice">温馨提示</div>
                    <ul>
                        <li>1<span>请填写证明内容</span></li>
                        <li>2<span>请上传相关证明</span></li>
                        <li>
                            3<span>提交后,工作人员会在3-5个工作日结算</span>
                        </li>
                    </ul>
                </div>
                <div class="com">
                    <h6>相关商品:</h6>
                    <span>{{ reportDetailData.title }}</span>
                </div>
                <div class="com">
                    <h6>举报类型:</h6>
                    <span>{{ reportDetailData.type }}</span>
                </div>
                <div class="com">
                    <h6>举报主题:</h6>
                    <span>{{ reportDetailData.topic }}</span>
                </div>
                <div class="com">
                    <h6>举报内容:</h6>
                    <span>{{ reportDetailData.content }}</span>
                </div>
                <div class="com">
                    <h6>图片展示:</h6>
                    <!--预览-->

                    <show-img :reportDetailData="reportDetailData"></show-img>
                </div>
            </div>
            <div class="input-wrapper">
                <h6>证明内容:</h6>
                <el-input
                    type="textarea"
                    :rows="4"
                    placeholder="请输入内容"
                    v-model="textarea"
                >
                </el-input>
            </div>
            <div class="select-wrapper">
                <h5>图片:</h5>
                <el-upload
                    :action="difficulty()"
                    list-type="picture-card"
                    :with-credentials="true"
                    :on-success="succeed"
                    :on-preview="handlePictureCardPreview"
                    :on-remove="handleRemove"
                    :on-exceed="beyond"
                    :limit="3"
                    name="adv_content"
                    accept=".jpg,.jpeg,.png,.gif,.JPG,.JPEG,.GIF,.PNG,"
                >
                    <i class="el-icon-plus"></i>
                </el-upload>
                <el-dialog :visible.sync="dialogVisible">
                    <img width="100%" :src="dialogImageUrl" alt="" />
                </el-dialog>
            </div>

            <el-button @click="confirm">确认提交</el-button>
        </div>
    </div>
</template>

<script>
import ShowImg from "./imgShow";
export default {
    data() {
        return {
            resStatus: "", //正确获取图片状态
            checkPic: [], //检查图片是否满足要求
            picArr: [], //接受多张图片
            figureCollection: [], //图片路径
            dialogImageUrl: "",
            dialogVisible: false,
            imgURL: "agent.shopsn.cn",
            textarea: "",
            imgIndex: -1,
            showImgClick: false
        };
    },

    props: {
        reportDetailData: {
            type: Object,
            default: null
        }
    },
    //生命周期 - 创建完成（访问当前this实例）
    created() {},
    //生命周期 - 挂载完成（访问DOM元素）
    mounted() {},
    computed: {
        handlePicture() {
            if (this.reportDetailData.pic_url) {
                return this.reportDetailData.pic_url.split(",");
            } else {
                return this.reportDetailData.pic_url;
            }
        }
    },
    methods: {
        handleItemCancel() {
            this.$emit("cancel");
        },
        //检验输入信息
        checkMessage() {
            if (this.textarea.match(/^\s*$/)) {
                this.$message.error("请输入举报内容");
                return;
            } else {
                this.$emit("goBackTop");
            }
        },
        confirm() {
            this.$HTTP(
                this.$httpConfig.commitReport,
                {
                    id: this.reportDetailData.goods_id,
                    pic_url: this.reportDetailData.pic_url
                },
                "post"
            )
                .then(res => {
                    if (res.data.status == 1) {
                        this.$message.success(res.data.message);
                        this.checkMessage();
                    }
                })
                .catch(err => {
                    console.error(err);
                });
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
        handlePictureCardPreview(file) {
            this.dialogImageUrl = file.url;
            this.dialogVisible = true;
            console.log(file, "file");
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
        }
    },
    components: {
        ShowImg
    }
};
</script>
<style lang="less">
.storeNoticeItem-wrapper {
    .el-upload__input {
        display: none;
    }
    .el-button {
        position: absolute;
        left: 50%;
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
    .show-detail {
        .container {
            h5 {
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
            .com {
                margin: 30px 0;
                h6 {
                    display: inline-block;
                    font-size: 16px;
                }
                span {
                    font-size: 16px;
                    margin-left: 5px;
                }
            }
        }
        .input-wrapper {
            margin-left: 30px;
            h6 {
                margin: 10px;
                font-size: 16px;
            }
        }
        .select-wrapper {
            margin-left: 30px;
            h5 {
                margin: 10px;
                font-size: 16px;
            }
        }
    }
}
</style>
