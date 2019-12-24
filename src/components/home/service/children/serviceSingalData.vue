<template>
    <div class="serviceSingalData-wrapper">
        <div class="message">
            <p>消息内容:</p>
            <span>{{ singalServiceData.sm_content }}</span>
        </div>
        <div class="time">
            <p>发送时间:</p>
            <span>{{ singalServiceData.sm_addtime | formatDate }}</span>
        </div>
        <el-button @click="back">返回上级</el-button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            singalServiceData: {}
        };
    },
    //生命周期 - 创建完成（访问当前this实例）
    created() {
        this.checkSingalData();
    },
    //生命周期 - 挂载完成（访问DOM元素）
    mounted() {},
    methods: {
        checkSingalData() {
            this.$HTTP(this.$httpConfig.serviceSingalData, {
                id: this.$route.query.id
            })
                .then(res => {
                    this.singalServiceData = res.data.data;
                    console.log(res.data.data);
                })
                .catch(err => {
                    console.error(err);
                });
        },
        back() {
            this.$router.push({
                name: "serviceMessage"
            });
        }
    },
    components: {}
};
</script>
<style lang="less">
.serviceSingalData-wrapper {
    .el-button  {
        position: absolute;
        left: 50%;
        top: 50%;
    }
}
</style>
<style lang="less" scoped>
.serviceSingalData-wrapper {
    .message {
        display: flex;
        align-items: center;
        margin: 10px;
        span {
            margin-left: 10px;
        }
    }
    .time {
        display: flex;
        align-items: center;
        margin: 10px;
        span {
            margin-left: 10px;
        }
    }
}
</style>
