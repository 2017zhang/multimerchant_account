<template>
    <div class="consult_wrapper">
        <h6>商品咨询</h6>
        <h5>查看全部咨询</h5>
        <div class="cousult">
            <input type="text" placeholder="请发表你的观点" v-model="inputValue" />
        </div>
        <div class="footter">
            <div class="com" v-for="(item,index) in dataList" :key="index">
                <span>Q</span>
                <div class="question">{{item.content}}</div>
                <div class="time_top">{{item.create_time|formatDate}}</div>
                <!-- <div class="answer" @click="answer(item)">我来回答</div> -->
                <div class="btn" @click="handleSubmit(item,index)">提交</div>
            </div>
        </div>
    </div>
</template>

<script>
import qs from "qs";
export default {
    data() {
        return {
            dataList: [],
            btnClick: false,
            inputValue: ""
        };
    },
    methods: {
        answer(item) {
            this.btnClick = true;
            this.getDataInfo(item);
        },
        handleSubmit(item, index) {
            if (this.inputValue) {
                this.inputValue = this.inputValue.replace(/\s*/g, "");
            } else if (
                this.inputValue == "" ||
                this.inputValue == undefined ||
                this.inputValue == null
            ) {
                alert("请先选择你要提问的问题");
                return;
            }
            this.$HTTP(this.$httpConfig.consultReply, {
                id: item.id,
                content: this.inputValue
            })
                .then(res => {
                    if (res.data.status) {
                        this.$message({
                            message: res.data.message,
                            type: "success"
                        });
                    }
                    this.inputValue = "";
                })
                .catch(err => {
                    console.error(err);
                });
        }
    },
    mounted() {
        this.$HTTP(this.$httpConfig.getConsultQuestion, {})
            .then(res => {
                this.dataList = res.data.data;
            })
            .catch(err => {
                console.error(err);
            });
    }
};
</script>

<style lang="less" scoped>
.consult_wrapper {
    padding: 20px;
    height: 100%;
    overflow-y: auto;
    h6 {
        border-bottom: 1px solid #e8e8e8;
        padding: 10px 0;
    }
    h5 {
        padding: 10px 0;
    }
    .cousult {
        display: flex;
        input {
            border: 1px solid #e8e8e8;
            width: 500px;
            height: 30px;
            border-radius: 6px;
        }
        .btn {
            width: 60px;
            height: 14px;
            background: #ff6100;
            text-align: center;
            line-height: 14px;
            cursor: pointer;
            color: #fff;
        }
    }
    .footter {
        .com {
            display: flex;
            position: relative;
            padding: 10px;
            .question {
                margin-left: 10px;
            }
            .time_top {
                position: absolute;
                left: 220px;
            }

            .answer {
                position: absolute;
                left: 400px;
                width: 60px;
                height: 14px;
                background: #ff6100;
                text-align: center;
                line-height: 14px;
                cursor: pointer;
                color: #fff;
            }
            .btn {
                position: absolute;
                left: 500px;
                background: #ff6100;
                text-align: center;
                line-height: 10px;
                cursor: pointer;
                color: #fff;
                height: 10px;
            }
        }
    }
}
</style>