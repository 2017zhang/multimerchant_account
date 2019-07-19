<template>
	<div class="logistics fl">
		<!-- 添加收货地址 -->
		<div class="t_tab">
			<h1 class="t_title">
				<span v-if="!status" class="size16">添加配送员</span>
				<span v-else class="size16">修改配送员</span>
				<div @click="to" class="t_m fr">返回配送员地址</div>
			</h1>
			<el-form ref="form" class="form" label-width="180px" :model="fromparams">
				<el-form-item label="账号">
					<el-input style="width: 600px;" v-model="fromparams.account" clearable></el-input>
				</el-form-item>
				<el-form-item label="姓名">
					<el-input style="width: 600px;" v-model="fromparams.name" clearable></el-input>
				</el-form-item>
				<el-form-item label="手机号">
					<el-input style="width: 600px;" v-model="fromparams.mobile" clearable></el-input>
				</el-form-item>
				<el-form-item label="登陆密码">
					<el-input style="width: 600px;" v-model="fromparams.password" clearable></el-input>
				</el-form-item>

				<el-form-item>
					<el-button type="success" @click="addDeliveryList">确认提交</el-button>
				</el-form-item>
			</el-form>
		</div>
	</div>
</template>
<script>
	import timePlunge from "../../../page/time";
	export default {
		name: 'ship',
		data() {
			return {
				fromparams: {
					account:'',
					name:'',
					mobile:'',
					password:''
				},
				params_status: false,
				status: 0, //0代表是新增，1代表是修改
				id: 0
			}
		},
		mounted() {

			this.status = this.$route.params.status;
			if(this.status) {
				/*修改*/
				this.id = this.$route.params.id;
				this.queryData();
			}
		},

		methods: {
			addDeliveryList(){
				this.$HTTP( this.$httpConfig.adddeliveryListList , this.fromparams,
						'post').then((res)=> {

				}).catch((e) =>  {
					console.log(e);
				})

			},
			getCityData(data){
				this.params.prov = data.prov;
				this.params.city = data.city;
				this.params.dist = data.dist;
			},
			to() {
				this.$router.back();
			},
			queryData() {
				this.$HTTP(this.$httpConfig.getAddressDetail,{id:this.id}).then((res) => {
					this.$forceUpdate();
					this.params = res.data.data;
					this.params_status = !Number(!Number(this.params.status));
					this.$message.success(res.data.message);
				})
			},
			submit() {
				var that = this;
				this.params.status = this.params_status;
				if(this.status) {
					let status = Number(this.params.status);
					this.params.status = status;
					this.$HTTP(this.$httpConfig.saveAddress,this.params).then((res) => {
						this.$message.success(res.data.message + ",即将跳转回列表页面");
						setTimeout(function() {
							that.to();
						}, 2000);
					})
							.catch((err) => {
								this.$message.error(err);
							});
				} else {
					let status = Number(this.params.status);
					this.params.status = status;
					this.$HTTP(this.$httpConfig.addAddress,this.params).then((res) => {
						this.$message.success(res.data.message + ",即将跳转回列表页面");
						setTimeout(function() {
							that.to();
						}, 2000);
					})
							.catch((err) => {
								this.$message.error(err);
							});
				}
			},
		},
	}
</script>
<style type="text/css" scoped="scoped">

</style>
<style lang="less">
	.t_tab {
		.t_title {
			color: #333;
			border-bottom: 1px solid #dddddd;
			overflow: hidden;
			margin-bottom: 22px;
			line-height: 50px;
			span {
				float: left;
				color: #333;
			}
			.t_m {
				background: url(../../../../assets/return.jpg) no-repeat 10px #ff9f24;
				width: 135px;
				height: 32px;
				border: 1px solid #ff920b;
				border-radius: 4px;
				font-size: 12px;
				font-weight: normal;
				cursor: pointer;
				color: #FFF;
				line-height: 32px;
				text-indent: 30px;
			}
		}
	}
</style>

<style lang="less" scoped>
	.logistics {
		width: 1000px;
	}
	.city{
		width:100px;
		height: 20px;
	}
</style>