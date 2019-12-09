<template>
    <div class="storeNoticeItem-wrapper">
        <div class="show-detail">
            <div class="container">
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
                    <div
                        class="img-wrapper"
                        v-for="(item, index) in handlePicture"
                        :key="index"
                    >
                        <img :src="URL + item" alt="" />
                    </div>
                </div>
            </div>
            <el-button @click="confirm">确认提交</el-button>
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
            dialogVisible: false,
            imgURL: "agent.shopsn.cn"
        };
    },
    computed: {
        handlePicture() {
            if (this.reportDetailData.pic_url) {
                return this.reportDetailData.pic_url.split(",");
            } else {
                return this.reportDetailData.pic_url;
            }
        }
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
    methods: {
        handleItemCancel() {
            this.$emit("cancel");
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
                        this.$emit("goBackTop");
                    }
                })
                .catch(err => {
                    console.error(err);
                });
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
                .img-wrapper {
                    margin: 10px 0;
                    img {
                        margin-left: 5px;
                        width: 200px;
                        height: 100px;
                    }
                }
            }
        }
    }
}
</style>
