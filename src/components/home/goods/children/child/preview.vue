<template>
    <div class="previewImg-wrapper"></div>
</template>

<script>
export default {
    data() {
        return {};
    },
    //生命周期 - 创建完成（访问当前this实例）
    created() {},
    //生命周期 - 挂载完成（访问DOM元素）
    mounted() {},
    methods: {},
    components: {}
};
</script>
<style lang="less">
.previewImg-wrapper {
}
</style>
<style lang="less" scoped>
.previewImg-wrapper {
    /*图片预览 缩略图*/
    .preview figure {
        float: left;
        width: 30%;
        height: calc(30vw - 0px);
        margin: 1.5%;
    }

    .preview figure img {
        width: 100%;
    }
}
</style>
