<template>
	<div class="currency">
		<div class="notes-box">
			<div class="notes_title">温馨提示</div>
			<div class="notes_content">
				<p>发布商品前必须添加对应分类的商品规格</p>
			</div>
		</div>
		<p class="d_title size14">店铺经营信息</p>
		<div class="z_info">
			<div class="goods_name">
				<span><b class="red">*</b> 商品名称： </span>
				<el-input v-model="title" :maxlength="60" size="medium" style="width: 250px;" clearable>
				</el-input>
				<p class="cue">请输入商品名,不能超过60个字符</p>
			</div>
			<div class="goods_brief">
				<span class="fl">商品简介：</span>
				<el-input style="width: 288px;" type="textarea" :maxlength="60" :autosize="{ minRows: 2, maxRows: 4}" v-model="description">
				</el-input>
				<p class="cue">请输入商品简介,不能超过60个字符</p>
			</div>
			<div class="goods_classify" style="margin: 10px 0;">
				<span><b class="red">*</b> 商品分类： </span>
				<el-select v-model="class_id" @change="selectChild()" filterable placeholder="请选择">
					<el-option v-for="item in classData" :key="item.id" :label="item.class_name" :value="item.id">
					</el-option>
				</el-select>
				<el-select @change="selectChildByTwo()" v-if="classByTwo.length !== 0" v-model="class_two" filterable placeholder="请选择">
					<el-option v-for="(items,indexs) in classByTwo" :key="indexs" :label="items.class_name" :value="indexs">
					</el-option>
				</el-select>
				<el-select v-if="classByThree.length !== 0" @change="va" v-model="class_three" filterable placeholder="请选择">
					<el-option v-for="(items,indexs) in classByThree" :key="indexs" :label="items.class_name" :value="indexs">
					</el-option>
				</el-select>
			</div>

			<div class="goods_classify" style="margin: 10px 0;">
				<span><b class="red">*</b> 店内分类： </span>
				<el-select v-model="store_class_id" @change="selectClsssChild()" filterable placeholder="请选择">
					<el-option v-for="item in storeClassData" :key="item.id" :label="item.class_name" :value="item.id">
					</el-option>
				</el-select>
				<el-select @change="selectClassChildByTwo()" v-if="storeclassByTwo.length !== 0" v-model="store_class_two" filterable placeholder="请选择">
					<el-option v-for="(items,indexs) in storeclassByTwo" :key="items.id" :label="items.class_name" :value="items.id">
					</el-option>
				</el-select>
				<el-select v-if="storeclassByThree.length !== 0" @change="va" v-model="store_class_three" filterable placeholder="请选择">
					<el-option v-for="(items,indexs) in storeclassByThree" :key="items.id" :label="items.class_name" :value="items.id">
					</el-option>
				</el-select>
			</div>

			<div class="goods_brand public">
				<span>商品品牌： </span>
				<el-select v-model="brand_id" placeholder="请选择" style="margin: 10px 0;">
					<el-option v-for="(item,index) in brandData" :key="index" :label="item" :value="index">
					</el-option>
				</el-select>
			</div>
			<div class="market_price z_price">
				<span><b class="red">*</b> 零售价：</span>
				<el-input v-model="price_market" size="medium" style="width: 250px;margin: 10px 0;" clearable>
				</el-input> 元
			</div>
			<!-- <div class="Member_price z_price">
				<span><b class="red">*</b> 会员价： </span>
				<el-input v-model="price_member" size="medium" style="width: 250px;margin: 10px 0;" clearable>
				</el-input> 元
			</div> -->


<div class="Member_price z_price">
				<span><b class="red">*</b> 批发价： </span>
				<el-input v-model="price_trade" size="medium" style="width: 250px;margin: 10px 0;" clearable>
				</el-input> 元
			</div>
            <div class="Member_price z_price">
				<span><b class="red">*</b> 起批价： </span>
				<el-input v-model="can_trade_price" size="medium" style="width: 250px;margin: 10px 0;" clearable>
				</el-input> 元
			</div>
            <div class="Member_price z_price">
				<span><b class="red">*</b> B团价： </span>
				<el-input v-model="price_b" size="medium" style="width: 250px;margin: 10px 0;" clearable>
				</el-input> 元
			</div>
            <div class="Member_price z_price">
				<span><b class="red">*</b> C团价： </span>
				<el-input v-model="price_c" size="medium" style="width: 250px;margin: 10px 0;" clearable>
				</el-input> 元
			</div>


            <div class="Member_price z_price" style="padding-left:0;">
				<span><b class="red">*</b> 生产日期：</span>
                <el-date-picker v-model="date_in_produced" type="date" placeholder="选择日" @change="dataSearch" value-format="yyyy-MM-dd" style="width: 250px;margin: 10px 0;"></el-date-picker>
				<!-- <el-input v-model="price_c" size="medium" style="width: 250px;margin: 10px 0;" clearable>
				</el-input>  -->
			</div>
            <div class="Member_price z_price">
				<span><b class="red">*</b> 保质期：</span>
                <el-date-picker v-model="goods_expiration_date" type="date" placeholder="选择日" @change="dataSearch" value-format="yyyy-MM-dd" style="width: 250px;margin: 10px 0;"></el-date-picker>
				<!-- <el-input v-model="price_c" size="medium" style="width: 250px;margin: 10px 0;" clearable>
				</el-input>  -->
			</div>
           
            <div class="goods_brand public">
				<span>商品标识： </span>
				<el-select v-model="mark_type" placeholder="请选择" style="margin: 10px 0;">
					<el-option v-for="(item,index) in brandData" :key="index" :label="item" :value="index">
					</el-option>
				</el-select>
			</div>

            <div class="goods_brand public">
				<span>产地证明： </span>
				<el-upload class="uploadImgBox"
                ref='pic_place_upload'
                    action="#"
                    multiple
                    :limit="1"
                    list-type="picture-card"
                    :before-upload="beforeAvatarUpload"
                    :auto-upload="false"
                    :on-change="handleChangeA"
                    :file-list="pic_placeList"
                    >
                        <i slot="default" class="el-icon-plus"></i>
                        <div slot="file" slot-scope="{file}">
                        <img
                            class="el-upload-list__item-thumbnail"
                            :src="file.url" alt=""
                        >
                        <span class="el-upload-list__item-actions">
                            
                            <span
                            v-if="!disabled"
                            class="el-upload-list__item-delete"
                            @click="handleRemove(file,'pic_place_upload')"
                            >
                            <i class="el-icon-delete"></i>
                            </span>
                        </span>
                        </div>
                    </el-upload>
			</div>

            <div class="goods_brand public">
				<span>合格证明： </span>
				<el-upload class="uploadImgBox"
                ref='pic_qualified_upload'
                    action="#"
                    multiple
                    :limit="1"
                    list-type="picture-card"
                    :auto-upload="false"
                    :before-upload="beforeAvatarUpload"
                    :on-change="handleChangeB"
                    :file-list="pic_qualifiedList">
                        <i slot="default" class="el-icon-plus"></i>
                        <div slot="file" slot-scope="{file}">
                        <img
                            class="el-upload-list__item-thumbnail"
                            :src="file.url" alt=""
                        >
                        <span class="el-upload-list__item-actions">
                            
                            <span
                            v-if="!disabled"
                            class="el-upload-list__item-delete"
                            @click="handleRemoveB(file)"
                            >
                            <i class="el-icon-delete"></i>
                            </span>
                        </span>
                        </div>
                    </el-upload>
			</div>
<div class="goods_brand public">
				<span>检查报告： </span>
				<el-upload class="uploadImgBox"
                ref='pic_examine_upload'
                    action="#"
                    multiple
                    :limit="1"
                    list-type="picture-card"
                    :auto-upload="false"
                    :on-change="handleChangeC"
                    :file-list="pic_examineList"
                    >
                        <i slot="default" class="el-icon-plus"></i>
                        <div slot="file" slot-scope="{file}">
                        <img
                            class="el-upload-list__item-thumbnail"
                            :src="file.url" alt=""
                        >
                        <span class="el-upload-list__item-actions">
                            
                            <span
                            v-if="!disabled"
                            class="el-upload-list__item-delete"
                            @click="handleRemoveC(file)"
                            >
                            <i class="el-icon-delete"></i>
                            </span>
                        </span>
                        </div>
                    </el-upload>
			</div>


			<div class="depot_num z_num">
				<span><b class="red">*</b> 仓库数量： </span>
				<el-input v-model="stock" size="medium" style="width: 250px;margin: 10px 0;" clearable>
				</el-input> 个
			</div>
			<div class="depot_num z_num">
				<span><b class="red">*</b> 商品重量：</span>
				<el-input v-model="weight" size="medium" style="width: 250px;margin: 10px 0;" clearable>
				</el-input> kg
			</div>
			<div class="goods_classify" style="margin: 10px 0;">
				<span><b class="red">*</b> 运费设置：</span>
				<el-select v-model="freight" placeholder="请选择" style="margin: 10px 0;">
					<el-option v-for="(item,index) in freightData" :key="index" :label="item" :value="index">
					</el-option>
				</el-select>
				<el-button @click.native="tolink" type="success">新建运费模板</el-button>
				<el-button @click.native="getFreight" type="success">重新加载</el-button>
			</div>
			<div class="set">
				<span>设置： </span>
				<!-- <input type="checkbox" v-model="shelves">
				上架 -->
				<input type="checkbox" v-model="recommend">
				推荐
			</div>
		</div>
		<!--<div class="presell_days">-->
			<!--<span><b class="red">*</b> 预售天数(库存为0)：</span>-->
			<!--<el-input v-model="advance_date" size="medium" style="width: 250px;margin: 10px 0;" clearable>-->
			<!--</el-input> 天-->
		<!--</div>-->
		<div class="editor">
			<span class="fl">商品详情描述： </span>
			<div class="ue fl">
				<UE :defaultMsg="defaultMsg" :config="config" ref="ue"></UE>
			</div>
		</div>
		<div class="button">
			<span @click="addGoods">确认提交</span>
		</div>
	</div>
</template>
<script>
import UE from './UE';
export default {
	name: 'currency',
	data() {
		return {
			defaultMsg: '',
			config: {
				initialFrameWidth: null,
				initialFrameHeight: 350
			},
			title: '',   // 商品名称
			description: '',  // 商品简介
			class_id: 0,    // 第一级分类
			class_two: 0,    // 第二级分类
			class_three: 0,    // 第三级分类编号
			store_class_id: 0, //店内第一级分类
			store_class_two: 0, //店内第二级分类
			store_class_three: 0, //店内第三级分类
			brand_id: 0,    // 品牌编号
			classData: {},   // 一级分类
			storeClassData:[], // 店内一级分类
			// price_market: 0.00, // 市场价
			price_member: 0.00,// 会员价

            mark_type:'',           //商品标示:
            price_market:0.00,         //零售价:
            price_trade: 0.00,         //批发价:
            can_trade_price:0.00,         //起批价:
            price_b:0.00,              //B团价:
            price_c:0.00,               // C团价:
            date_in_produced:'',         //生产日期:
            goods_expiration_date:'',      //保质期:
            pic_place:'123',               // 产地证明:
            pic_qualified:'',           // 合格证明:
            pic_examine:'',             // 检查报告:
            pic_placeList:[],               // 产地证明:
            pic_qualifiedList:[],           // 合格证明:
            pic_examineList:[],             // 检查报告:
            
         dialogImageUrl: '',
        dialogVisible: false,
        disabled: false,

			advance_date: 0,  // 预售期
			shelves: 0,  // 是否上架
			recommend: 0,  // 是否推荐
			stock: 0,  // 库存
			detail: '', //商品详情
			brandData: {}, // 品牌数据
			classByTwo: [], // 二级分类数据
			storeclassByTwo: [], // 店内二级分类数据
			classByThree: [],  // 三级分类数据
			storeclassByThree: [],  // 店内三级分类数据
			weight: 0.0, // 重量 以克为计量单位
			freight: '',		//运费模板
			freightData: [],
            addGoodsBaseData:[]

		};
	},
	props: [],
	created() {
		//是否登录
		console.log(this)
		this.getClass();
		this.getBrand();
		this.getFreight();
		this.getOneStoreClass(); //获取一级店内分类
        this.initData({});
        this.GetGoodsBase();
	},

	methods: {
		tolink() {
			const { href } = this.$router.resolve({
				name: 'logistics'
			})
			window.open(href, '_blank')
		},
		getOneStoreClass(){
			this.$HTTP(this.$httpConfig.storeClassList,{},'post').then(res=>{
				console.log(res);
				this.storeClassData = res.data.data;
			}).catch(err=>{
				console.log(err);
			})
		},
		getUEContent() {
			let content = this.$refs.ue.getUEContent();
		//	console.log(content);
			return content;
		},
		//给全局变量赋值
		va() {
			this.$store.state.three_class_id = this.class_three;
		},
		// 获取 品牌
		getBrand() {
			this.$HTTP(this.$httpConfig.getBrandList, {}, 'post').then((res) => {
				this.brandData = res.data.data;
			})
		},
		//获取运费
		getFreight() {
			this.$HTTP(this.$httpConfig.getGoodsFreightList, {}, 'post').then((res) => {
				this.addGoodsBaseData = res.data.data;
                console.log(this.addGoodsBaseData);
			}).catch((err) => {
				this.$message.error(err.data.message);
			});
		},
		//商品分类搜索
		getClass() {
			this.$HTTP(this.$httpConfig.getClassById, {
				goods_class_id: parseInt(this.class_id)
			}, 'post').then((res) => {
				if (!res.data.data) {
					this.$layer.msg(res.data.message);
					return;
				}
				this.classData = res.data.data;
			}).catch((err) => {
				console.log(err)
			});
		},
		/**
		 * 二级分类（change);
		 */
		selectChild() {
			this.class_two = null
			this.classByTwo = [];
			this.classByThree = [];
			this.class_three = null;
			console.log(this.class_id);
			//获取分类
			this.$HTTP(this.$httpConfig.getClassById, {
				goods_class_id: parseInt(this.class_id)
			}, 'post').then((res) => {
				if (!res.data.data) {
					this.$layer.msg(res.data.message);
					return;
				}
				this.classByTwo = res.data.data;
			//	console.log(this.classByThree)
			}).catch((err) => {
				console.log(err)
			});
		},

		selectClsssChild(){
			this.store_class_two = null
			this.storeclassByTwo = [];
			this.storeclassByThree = [];
			this.store_class_three = null;
			this.$HTTP(this.$httpConfig.nextClass, {
				id: parseInt(this.store_class_id)
			}, 'post').then((res) => {
				if (!res.data.data) {
					this.$layer.msg(res.data.message);
					return;
				}
				this.storeclassByTwo = res.data.data;
				//	console.log(this.classByThree)
			}).catch((err) => {
				console.log(err)
			});
		},
		/**
		 * 第三级分类
		 */
		selectChildByTwo() {
			this.classByThree = [];
			this.class_three = null;
			console.log(this.class_two,'class_two');
			//获取分类
			this.$HTTP(this.$httpConfig.getClassById, {
				goods_class_id: parseInt(this.class_two)
			}, 'post').then((res) => {
				if (!res.data.data) {
					this.$layer.msg('暂无数据:(');
					return;
				}
				this.classByThree = res.data.data;
			}).catch((err) => {
				console.log(err)
			});
		},
		selectClassChildByTwo(){
			this.storeclassByThree = [];
			this.store_class_three = null;
			this.$HTTP(this.$httpConfig.nextClass, {
				id: parseInt(this.store_class_two)
			}, 'post').then((res) => {
				if (!res.data.data) {
					this.$layer.msg('暂无数据:(');
					return;
				}
				this.storeclassByThree = res.data.data;
			}).catch((err) => {
				console.log(err)
			});
		},
        warningOpen(text) {
            this.$message({
            message: text,
            type: 'warning'
            });
        },
        // 商品标识 addGoodsBase
            GetGoodsBase() {
                console.log("商品标识")
			this.$HTTP(this.$httpConfig.addGoodsBase, {}, 'post').then((res) => {
				this.addGoodsBaseData = res.data.data;
                console.log(this.addGoodsBaseData)
			}).catch((err) => {
                console.log("失败-商品标识")
				this.$message.error(err.data.message);
			});
		},
		/**
		 * 添加商品
		 */
		addGoods() {

			let dataJson = {
				'id': 0,
				'class_id': this.class_id,
				'class_two': this.class_two,
				'class_three': this.class_three,
				'brand_id': this.brand_id,
				'recommend': Number(this.recommend),
				'shelves': Number(this.shelves),
				'advance_date': this.advance_date,
				'stock': this.stock,
				// 'price_market': this.price_market,
				'price_member': this.price_member,
				'weight': this.weight,
				'express_id': this.freight,
				'store_class_one': this.store_class_id,
				'store_class_two': this.store_class_two,
				'store_class_three' : this.store_class_three,

            
            'price_market':this.price_market,         //零售价:
            'price_trade':this.price_trade,         //批发价:
            'can_trade_price':this.can_trade_price,         //起批价:
            'price_b':this.price_b,              //B团价:
            'price_c':this.price_c,               // C团价:
            

			};
            console.log("商品标示:"+this.mark_type);
            console.log("零售价:"+this.price_market);
            console.log("批发价:"+this.price_trade);
            console.log("起批价:"+this.can_trade_price);
            console.log("B团价:"+this.price_b);
            console.log("C团价:"+this.price_c);
            console.log("生产日期:"+this.date_in_produced);
            console.log("保质期:"+this.goods_expiration_date)
            console.log("产地证明:"+this.pic_place)
            console.log("合格证明:"+this.pic_qualified)
            console.log("检查报告:"+this.pic_examine)
			for (let i in dataJson) {
			//	console.log(dataJson[i]);
				if (isNaN(dataJson[i])) {
					// this.$layer.msg('数据不合法');
                    console.log(dataJson[i])
                    this.warningOpen("数据不合法");
					return;
				}
			}
             console.log("提交")
			if (!/^[\u4e00-\u9fa5\s_a-zA-Z0-9/_、，；！：,. 。“”【】（）*+-]+$/.test(this.title)) {
				// this.$layer.msg('商品名称异常，请尽量输入中文符号');
                this.warningOpen("商品名称异常，请尽量输入中文符号");
				return false;
			}
			// if (!/^[\^[a-zA-Z\u4e00-\u9fa5]+$/.test(this.title)) {
			// 	this.$layer.msg('商品名称异常，请尽量输入中文符号');
			// 	return false;
			// }
            if(!this.mark_type){
                 this.warningOpen("商品名称异常，请尽量输入中文符号");
				return false;
            }

            
			dataJson['title'] = this.title;
			dataJson['detail'] = this.getUEContent();
			dataJson['description'] = this.description;
            // 非数字
            dataJson['mark_type'] = this.mark_type,           //商品标示:
            dataJson['date_in_produced'] = this.date_in_produced,         //生产日期:
            dataJson['goods_expiration_date'] = this.goods_expiration_date,      //保质期:
            dataJson['pic_place'] = this.pic_place,               // 产地证明:
            dataJson['pic_qualified'] = this.pic_qualified,           // 合格证明:
            dataJson['pic_examine'] = this.pic_examine,             // 检查报告:
           


			console.log(dataJson);
			// 提交商品基本信息
			this.$HTTP(this.$httpConfig.addGoods, dataJson, 'post').then((res) => {
				this.$message.success(res.data.message);
				if (!this.$store.state.tab_Index[1]) {
					this.$store.state.tab_Index.push(1);
				}
				this.$emit('selectTab');
				this.$store.state.tab_state = 1;
			}).catch((err) => {
				console.log(err)
			});
		},
        // 日期
        dataSearch() {
      this.getListByDay();
    },
    async initData(data) {
      //获取当前时间
      var now   = new Date();
      var monthn = now.getMonth()+1;
      var yearn  = now.getFullYear();
      var dayn = now.getDate();
      this.selectDay = yearn+"-"+monthn+"-"+dayn;

      this.selectUser = parseInt(sessionStorage.getItem("userid"));
      this.getListByDay();
    },
    async getListByDay(data) {

    },
     handleRemove(file) {
        // this.dialogVisible = false;
        this.$refs.pic_place_upload.clearFiles();
        // this.$refs.upload.clearFiles();
      },
      handleRemoveB(file) {
        this.$refs.pic_qualified_upload.clearFiles();
      },
      handleRemoveC(file) {
        this.$refs.pic_examine_upload.clearFiles();
      },
       
        handleChangeA(file,data){
            let path = data[0].url;
            let ext = data[0].raw.type; 
            let _this = this;
            this.getUrlBase64(path, ext, function (base64) {
        _this.pic_place = base64;
        });
        },
         
        handleChangeB(file,data){
            let path = data[0].url;
            let ext = data[0].raw.type;
            let _this = this;
            this.getUrlBase64(path, ext, function (base64) {
            console.log(base64);//base64编码值
            _this.pic_qualified = base64;        // // 合格证明                    
        });
        },
        handleChangeC(file,data){
            let path = data[0].url;
            let ext = data[0].raw.type;
            let _this = this;
            this.getUrlBase64(path, ext, function (base64) {
            console.log(base64);//base64编码值
            _this.pic_examine = base64;        // // 检查报告                      
        });
        },


      handlePictureCardPreview(file) {
        this.dialogImageUrl = file.url;
        console.log(this.dialogImageUrl)
        this.dialogVisible = true;
      },
       beforeAvatarUpload(file) {
           console.log("001");
        const isJPG = file.type === 'image/jpeg';
        const isLt2M = file.size / 1024 / 1024 < 2;

        if (!isJPG) {
              this.warningOpen("上传头像图片只能是 JPG 格式!");
        }
        if (!isLt2M) {
          this.warningOpen('上传头像图片大小不能超过 2MB!');
        }
        return isJPG && isLt2M;
      },
    //   图片转换base64
    getUrlBase64(url, ext, callback) {
    var canvas = document.createElement("canvas");   //创建canvas DOM元素
    var ctx = canvas.getContext("2d");
    var img = new Image;
    img.crossOrigin = 'Anonymous';
    img.src = url;
    img.onload = function () {
        // canvas.height = 60; //指定画板的高度,自定义
        // canvas.width = 85; //指定画板的宽度，自定义
        ctx.drawImage(img, 0, 0, 60, 85); //参数可自定义
        var dataURL = canvas.toDataURL("image/" + ext);
        callback.call(this, dataURL); //回掉函数获取Base64编码
        canvas = null;
        return dataURL;
    };
}
	},
	components: {
		UE
	},
}
</script>
<style>
body {
  overflow: inherit;
}
</style>
<style lang="less" scoped>
.uploadImgBox{
        border: 1px solid;
    display: inline-block;
    margin-left: 40px;
    margin-bottom:20px;
}
// .currency .z_info .public span, .currency .z_info .z_num span, .currency .z_info .z_price span{
//     border: 1px solid;
//     min-width: 57px;
//     display: inline-block;
// }
.currency .z_info .public .uploadImgBox .el-upload-list__item-preview span{
    width: auto;
    padding:0;
}
.currency {
  width: 100%;
  height: auto;
  min-height: 100px;
  line-height: 20px;
  padding: 10px;
  border-top: 0;
  color: #333;
  .d_title {
    padding: 10px 0;
  }
  .ss {
    padding-left: 5px;
  }
  .z_info {
    padding-left: 65px;
    border-radius: 4px;
    .goods_name {
      span {
        padding: 10px 0;
        b {
          color: #ff0000;
        }
      }
      .text_name {
        margin: 10px 0;
        width: 288px;
        height: 32px;
        border: 1px solid #cccccc;
        border-radius: 4px;
      }
      .cue {
        padding: 10px 0 10px 75px;
      }
    }
    .goods_brief {
      padding-left: 12px;
      span {
        padding: 10px 0;
      }
      textarea {
        border: 1px solid #cccccc;
        margin: 10px 0;
      }
      .cue {
        padding: 10px 0 10px 63px;
      }
    }
    .goods_classify {
      span {
        padding: 10px 0;
        b {
          color: #ff0000;
        }
      }
      select {
        margin: 10px 0;
        width: 288px;
        height: 32px;
        border: 1px solid #cccccc;
        border-radius: 4px;
      }
    }
    .public {
      padding-left: 12px;
      span {
        padding: 10px 0;
      }
      select {
        margin: 10px 0;
        width: 288px;
        height: 32px;
        border: 1px solid #cccccc;
        border-radius: 4px;
      }
    }
    .z_price {
      padding-left: 12px;
      border-radius: 4px;
      span {
        padding: 10px 0;
        b {
          color: #ff0000;
        }
      }
      input {
        margin: 10px 0;
        width: 288px;
        height: 32px;
        border: 1px solid #cccccc;
        border-radius: 4px;
      }
    }
    .z_num {
      span {
        padding: 10px 0;
        b {
          color: #ff0000;
        }
      }
      input {
        margin: 10px 0;
        width: 288px;
        height: 32px;
        border: 1px solid #cccccc;
        border-radius: 4px;
      }
    }
    .set {
      padding-left: 33px;
      span {
        padding: 10px 0;
      }
      input {
        margin: 10px 0;
        border: 0;
        width: 15px;
        height: 15px;
        border-radius: 4px;
      }
    }
  }
  .presell_days {
    padding-left: 12px;
    span {
      padding: 10px 0;
      b {
        color: #ff0000;
      }
    }
    input {
      margin: 10px 0;
      width: 288px;
      height: 32px;
      border: 1px solid #cccccc;
      border-radius: 4px;
    }
  }
  .editor {
    padding: 10px 0 0 50px;
    overflow: hidden;
    span {
      padding: 150px 0;
    }
    .ue {
      width: 800px;
    }
  }
  .button {
    height: 32px;
    margin: 20px 0 20px 135px;
    border-radius: 6px;
    background: #f7bc0a;
    width: 199px;
    span {
      display: block;
      line-height: 32px;
      color: #fff;
      text-align: center;
      cursor: pointer;
    }
  }
}
</style>
