<template>
	<!-- 店铺 -店铺设置 -->
	<div class="classify">
		<div class="g_att">
			<h1 class="t_title">
				<span class="size16">店铺销售区域</span>
			</h1>
			<div class="notes-box">
				<div class="notes_title">温馨提示</div>
				<div class="notes_content">
					<p>您填写的信息将在店铺前台展示给买家，请认真填写</p>
<!--				    <p>请在2分钟内添加图片，否则保存后点击“确认提交”按钮重新进入此页面修改。</p>-->
				</div>
				<div class="info-s">s-h-o-p-s-n</div>
			</div>
			<h1 class="t_title heading">
				<span class="size14">选择店铺区域</span>
			</h1>

			<table width="100%" border="0" cellspacing="0" cellpadding="0" class="manage list">

				<tr>
					<td align="right" class="black">
						<b></b>  </td>
					<td>
						<city-three v-on:sendcityData="getCityData" :sendParams="city_tree"></city-three>
					</td>
					<td class="submit-box">
						<el-button type="success" @click="submit">确认提交</el-button>
					</td>
				</tr>


			</table>
			<div class="addresslist">
				<div class="address" v-for="(item ,index) in Address_list" :key="index">
					<div>
						<img src="../../../../assets/close.png" @click="del(index)"><span>{{item}}</span>
					</div>
				</div>
				<div class="clearb"></div>
			</div>
		</div>
	</div>
</template>
<script>
export default {
	name: 'shop',
	data() {
		return {
			Address_list:{},
			city_tree: {},
			sales_area:[]


		}
	},
	created() {
		this.first_request();
	},
	mounted() {

	},
	methods: {
		//删除地区数据
		del(id){
			console.log(id)
			// this.$HTTP(this.$httpConfig.addSalesArea,this.sales_area,'post').then(res => {
			// 	console.log(res);
			// });
		},
		// 获取省市区数据
		getCityData(data) {
			this.sales_area["prov_id"] = data.prov;
			this.sales_area["city"] = data.city;
			this.sales_area["dist"] = data.dist;
		},
		//提交数据
		submit(){
			if(!this.sales_area["city"]) {
				alert("请选择地区")
				return
			};
			// this.$HTTP(this.$httpConfig.addSalesArea,this.sales_area,'post').then(res => {
			// 	console.log(res);
			// });
			console.log(this.sales_area,999000);
			this.sales_area=[];

		},
		//加载页面数据
		first_request(){
			this.$HTTP(this.$httpConfig.StoreSalesarea, {}).then(res => {
				// console.log(res,880000);
				if(res.status==200){
					// console.log(res.data.data)
					this.Address_list=res.data.data;
					console.log( this.Address_list[17],88888);
				}
			});
		}
	}
}
</script>
<style>
.classify {
  padding-bottom: 50px;
}
.submit-box {
  text-align: center;
}
.updata_image {
  margin-left: 10px;
}
.el-upload--picture-card i {
  line-height: 148px;
}
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #ff920b;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
.el-upload__input {
  display: none !important;
}
</style>
<style lang="less">
body {
  overflow: inherit;
}
.g_att {
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
    .t_add {
      background: url(../../../../assets/plus2.jpg) no-repeat 15px #ff9f24;
      width: 135px;
      height: 32px;
      border: 1px solid #ff920b;
      border-radius: 4px;
      font-size: 12px;
      font-weight: normal;
      cursor: pointer;
      color: #fff;
      line-height: 32px;
      text-indent: 35px;
    }
  }
  .heading {
    border: none;
    line-height: 30px;
    margin: 10px 0 10px 10px;
  }
  .el-date-editor.el-input,
  .el-date-editor.el-input__inner {
    width: 100px;
  }
}

.el-switch {
  .el-switch__core {
    width: 35px !important;
    height: 16px;
    .el-switch__button {
      width: 11px;
      height: 11px;
    }
  }
  .el-switch__label {
    color: #a4a5a7;
  }
  .el-switch__label.is-active {
    color: #f7bc0a;
  }
  .el-switch__label span {
    line-height: 20px;
    font-size: 12px;
  }
}
</style>
<style type="text/css">
#address .el-input {
  width: 500px;
}

.el-table td,
.el-table th.is-leaf {
  text-align: center;
}
</style>
<style lang="less" scoped>
	.clearb{
		clear: both;
	}
	.addresslist{
		margin-top: 25px;
		min-height:150px;
		width: 100%;
		box-sizing: border-box;
		border:1px solid #f5f5f5;
		padding:20px;
		.address{
			margin-left: 30px;
			float: left;
			width: 200px;
			margin-bottom: 15px;
			div{
				position: relative;
				background: #f5f5f5;
				padding:10px;
				border-radius: 5px;
				color:#333;
				span{
					margin-right:5px;
				}
				img{
					width: 18px;
					position: absolute;
					right: 0;
					top:0px;

				}
			}

		}
	}
.classify {
  width: 1000px;
  .g_att {
    .info {
      .el-input {
        width: 500px;
      }
    }
    .list {
      margin-top: 15px;
      tr {
        td {
          padding: 15px 0;
          input,
          select {
            padding-left: 5px;
            width: 288px;
            height: 32px;
            border: 1px solid #cccccc;
          }
          select {
            width: 180px;
          }
          .none {
            padding: 0;
            height: 80px;
          }
          .none_z {
            height: 50px;
            padding: 0;
          }
          .none_x {
            height: 35px;
            padding: 0;
          }
          img {
            background-color: #fbfdff;
            border: 1px dashed #c0ccda;
            border-radius: 6px;
            box-sizing: border-box;
            width: 148px;
            height: 148px;
            line-height: 146px;
            vertical-align: top;
            float: left;
            margin-right: 5px;
          }
          p {
            padding: 10px 0;
          }
          b {
            color: #ff0000;
          }
          textarea {
            padding-left: 5px;
            padding-top: 5px;
            min-height: 80px;
            border: 1px solid #cccccc;
          }
          .Settled {
            height: 32px;
            line-height: 32px;
            color: #fff;
            text-align: center;
            display: block;
            border-radius: 6px;
            margin-top: 20px;
            float: left;
            margin-right: 20px;
            cursor: pointer;
          }
        }
      }
    }
    .manage {
      margin-top: 15px;
      tr {
        td {
          padding: 15px 0;
          .time {
            border-left: 1px solid #ddd;
            border-right: 1px solid #ddd;
            border-top: 1px solid #ddd;
            tr {
              td {
                border-bottom: 1px solid #ddd;
                padding: 10px 0;
                .up_time {
                  padding-left: 10px;
                }
              }
              .right-border {
                border-right: 1px solid #ddd;
              }
            }
          }
        }
      }
    }
  }
  .info-s {
    display: none;
  }
}
</style>
