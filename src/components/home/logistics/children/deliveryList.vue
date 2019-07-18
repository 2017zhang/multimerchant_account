<template>

  <el-card class="box-card">
    <div slot="header" class="clearfix">
      <span>添加快递公司</span>
      <!--<el-button style="float: right; padding: 3px 0" type="text">操作按钮</el-button>-->
    </div>
    <div  class="text item">
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="公司名称">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="编号">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="首字母">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="公司网址">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="客服电话">
          <el-input v-model="form.name"></el-input>
        </el-form-item>

        <el-form-item label="折扣">
          <el-input v-model="form.name"></el-input>
        </el-form-item>

        <el-form-item label="是否启用">
          <el-switch v-model="form.delivery"></el-switch>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="delierySubmit">立即添加</el-button>
          <el-button>取消</el-button>
        </el-form-item>
      </el-form>
    </div>
  </el-card>

</template>
<script>
export default {
  name: "deliveryList",
  data() {
    return {

      form: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      }
};
},
  methods: {
    delierySubmit() {
      console.log('submit!');
    }
  }
};
</script>
<style >

</style>