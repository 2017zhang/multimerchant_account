<template>
  <div class="standard">
    <div class="notes-box">
      <div class="notes_title">温馨提示</div>
      <div class="notes_content">
        <p>必须添加商品规格才能在移动端和PC端显示商品</p>
      </div>
    </div>
    <div class="specification">
      <p class="goods_spec">商品规格：</p>
      <div class="g_size" v-for="(item, index) in group" :key="index">
        <span class="fl">{{item.name}}：</span>
        <ul class="fl spec_list">
          <li
            v-for="(spec,indexByItem) in select_list"
            v-if="index == spec.spec_id"
            :key="spec.id"
            :class="{color:item_id[indexByItem] == spec.id }"
            @click="tab1(indexByItem, spec, index,$event)"
            :data-spec-id="spec.spec_id"
            :data-item-id="spec.id"
          >{{spec.item}}</li>

          <div class="edit_btn">
            <div v-if="!item.btn">
              <el-button
                style="margin-left:20px"
                type="info"
                plain
                @click="addStyle(item,index)"
              >＋添加规格值</el-button>
            </div>
            <div v-if="item.btn">
              <input placeholder="规格值名称" class="edit_input" v-model="item.spec_name" />
              <el-button size="mini" type="primary" @click="confirm(item,index)">确认</el-button>
              <el-button size="mini" type="danger" @click="cancel(item,index)">取消</el-button>
            </div>
          </div>
        </ul>
      </div>
    </div>
    <div class="g_editss"></div>
    <div class="tijiao">
      <span @click="submit">确认提交</span>
    </div>
  </div>
</template>
<script>
import qs from "qs";
import Vue from "vue";
export default {
  name: "standard",
  data() {
    return {
      specResult: {},
      select_list: {},
      dataBySpec: "",
      spec_select: [],
      item_id: [],
      spec_select_tow: [],
      spec_data: "",
      group: "",
      inputList: {},
      btn: false,
      spec_name: ""
    };
  },
  props: {
    specData: null,
    s: ""
  },
  watch: {
    specData() {
      if (this.specData) {
        this.getData(this.specData);
      }
    }
  },
  methods: {
    getData(data) {
      console.log(data);
      if (this.spec_data && this.s == 2) return;
      console.log(12323)
      this.spec_data = data;
      let item_id = data.item_id;
      this.group = data.specList.group;
      let item = data.item_id;
      let children = data.specList.children;
      //处理
      for (let key in children) {
        this.select_list[children[key].id] = children[key];
      }
      for (let key in item_id) {
        this.item_id[item_id[key]] = item_id[key];
      }
      for (let i in item) {
        for (let index in children) {
          if (item[i] == children[index].id) {
            let spec_list = children[index].spec_id;
            if (typeof this.spec_select[spec_list] === "undefined") {
              this.spec_select[spec_list] = [];
            }
            this.spec_select[spec_list].push(item[i]);
          }
        }
      }
      console.log(this.select_list);
      this.getSpecItem();
    },
    //提交
    submit() {
      let that = this;
      let inputList = {};
      let flag = false;
      let tmp = {};
      let key;
      $("#spec_input_tab tr ").each(function(index, ele) {
        $(ele)
          .find("input")
          .each(function(index, ele) {
            key = $(ele).attr("data-key");
            let name = $(ele).attr("name");
            let val = $(ele).val();
            tmp[name] = val;
            //	console.log(val)
            inputList[key] = {};
            if (val === "") {
              flag = false;
            }
          });
        if (typeof key !== "undefined") {
          inputList[key] = tmp;
        }
        tmp = {};
      });
      if (flag === true) {
        this.$message({
          duration: 1000,
          type: "error",
          message: "规格信息错误"
        });
      } else {
        this.$HTTP(this.$httpConfig.editGoodsSpec, { item: inputList }, "post")
          .then(res => {
            this.$message({
              duration: 1000,
              message: res.data.message
            });
            this.$emit("selectTab");
            this.$emit("goodsParameter");
            this.$store.state.tab_state = 2;
            if (!this.$store.state.tab_Index[2]) {
              this.$store.state.tab_Index.push(2);
            }
          })
          .catch(err => {
            console.log(err);
          });
      }
    },
    //样式切换
    styleTab(ele) {
      if ($(ele).hasClass("color")) {
        $(ele).removeClass("color");
      } else {
        $(ele).addClass("color");
      }
    },
    //选择规格
    tab1(index, spec, specGroupId, event) {
      this.specResult = {};
      let ele = event.currentTarget;
      let _this = this;
      this.styleTab(ele);
      $(".spec_list li").each(function() {
        if ($(this).hasClass("color")) {
          let spec_id = $(this).data("spec-id");
          let item_id = $(this).data("item-id");
          if (!_this.specResult.hasOwnProperty(spec_id))
            _this.specResult[spec_id] = [];
          _this.specResult[spec_id].push(item_id);
        }
      });
      let status = false;
      this.getSpecItem(status);
    },
    /**
     * 合并单元格
     */
    mergeCell(id) {
      var tab = document.getElementById(id); //要合并的tableID
      var maxCol = 2,
        val,
        count,
        start; //maxCol：合并单元格作用到多少列
      if (tab != null) {
        for (var col = maxCol - 1; col >= 0; col--) {
          count = 1;
          val = "";
          for (var i = 0; i < tab.rows.length; i++) {
            if (val == tab.rows[i].cells[col].innerHTML) {
              count++;
            } else {
              if (count > 1) {
                //合并
                start = i - count;
                tab.rows[start].cells[col].rowSpan = count;
                for (var j = start + 1; j < i; j++) {
                  tab.rows[j].cells[col].style.display = "none";
                }
                count = 1;
              }
              val = tab.rows[i].cells[col].innerHTML;
            }
          }
          if (count > 1) {
            //合并，最后几行相同的情况下
            start = i - count;
            tab.rows[start].cells[col].rowSpan = count;
            for (var j = start + 1; j < i; j++) {
              tab.rows[j].cells[col].style.display = "none";
            }
          }
        }
      }
    },
    //获取规格项
    getSpecItem(status) {
      let spec = null;
      if (Object.keys(this.specResult).length !== 0) {
        spec = this.specResult;
      } else {
        if (Object.keys(this.specResult).length === 0 && status === false) {
          spec = spec;
        } else {
          spec = this.spec_select;
        }
      }
      this.$HTTP(this.$httpConfig.getGoodsSpec, { spec: spec }, "post")
        .then(res => {
          $(".g_editss").html("");
          $(".g_editss").append(res.data.data);
          let id = "spec_input_tab";
          this.mergeCell(id);
          this.$nextTick(() => {
            this.addUploadImg();
          });
        })
        .catch(err => {
          console.log(err);
        });
    },
    //添加图片
    addUploadImg() {
      let _this = this;
      $(".uploadImg").on("change", function(e) {
        _this.uploadImg(e.target.files[0], e.target);
      });
      $(".del").on("click", function(e) {
        _this.delGuige(e);
      });
    },
    delGuige(e) {
      //delChildGoods
      this.$HTTP(
        this.$httpConfig.delChildGoods,
        { id: e.target.dataset.value },
        "post"
      ).then(res => {
        console.log(res);
        if (res.data.status == 1) {
          this.$message.success(res.data.message);
          $(e.target)
            .parent()
            .parent()
            .remove();
        }
      });
    },
    //图片上传
    uploadImg(file, _dom) {
      const loading = this.$loading({
        lock: true,
        text: "Loading",
        spinner: "el-icon-loading",
        background: "rgba(0, 0, 0, 0.7)"
      });
      this.axios
        .post(
          this.$store.state.image_api_url + "FileUpload/getGoodsImageConfig",
          qs.stringify({})
        )
        .then(res => {
          if (res.data.status == 0) {
            this.$layer.msg(data.message);
            loading.close();
            return;
          }
          let data = res.data.data;
          this.imageConf = data.config;
          this.imageToken = data.token;
          this.sToken = data.s_token;

          var formData = new FormData();
          formData.append("fileData", file);
          formData.append("imageToken", this.imageToken);
          formData.append("sessionToken", this.sToken);
          this.axios
            .post(
              this.$store.state.image_api_url +
                "FileUpload/uploadImageToLocal/",
              formData
            )
            .then(res => {
              let _data = res.data;
              if (_data.status == 1) {
                this.$message.success(_data.message);
                $(_dom)
                  .siblings("input[type='hidden']")
                  .val(_data.data);
                $(_dom)
                  .parent()
                  .parent()
                  .siblings(".showImg")
                  .find("img")
                  .attr("src", this.URL + _data.data);
                loading.close();
              } else {
                this.$message.warning(_data.message);
                loading.close();
              }
            });
        });
    },
    addStyle(item, index) {
      Vue.set(this.group[index], "btn", true);
    },
    //确认
    confirm(item) {
      if (item.spec_name) {
        var str = item.spec_name.replace(/\s+/g, "");
        if (str == "") {
          this.$message("输入信息不能为空");
          return "";
        } else {
          this.$HTTP(this.$httpConfig.confirmGoodsItem, {
            spec_id: Number(item.id),
            item: str
          }).then(res => {
            if (res.data.status === 1) {
              this.$HTTP(this.$httpConfig.getGoodsSpecDetail, {
                class_three: this.$store.state.three_class_id_tow,
                goods_id: this.$route.params.id
              }, 'post').then(res => {
                this.spec_data = "";
                this.getData(res.data.data);
              }).catch((err) => {
                console.log(err);
              })
            }
          });
        }
      } else {
        this.$message("输入信息不能为空");
        return "";
      }
    },
    //取消
    cancel(item, index) {
      item.btn = false;
      Vue.set(this.group[index], "btn", false);
    }
  }
};
</script>
<style>
.g_editss .table {
  border-left: 1px solid #ddd;
  border-right: 1px solid #ddd;
  border-top: 1px solid #ddd;
  margin-top: 15px;
}
.g_editss .table tr td {
  padding: 10px 0;
  border-bottom: 1px solid #ddd;
  text-align: center;
  vertical-align: inherit;
}
.g_editss .table tr td b {
  font-weight: bold;
  color: #333;
}
.g_editss .table tr td input {
  padding-left: 5px;
  height: 32px;
  border: 1px solid #cccccc;
}
.g_editss .table tr td span {
  background: #088be6;
  display: inline-block;
  color: #ffffff;
  font-size: 12px;
  border-radius: 4px;
  line-height: 24px;
  width: 58px;
  height: 24px;
  position: relative;
  bottom: -1px;
  z-index: -1;
}
.uploadImg {
  width: 58px;
  height: 24px;
  border: none;
  background: #088be6;
  color: #ffffff;
  font-size: 12px;
  border-radius: 4px;
  opacity: 0;
  position: absolute;
}
.uploadBtn {
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  left: 0;
  opacity: 0;
}
</style>

<style lang="less" scoped>
.standard {
  height: auto;
  min-height: 100px;
  line-height: 20px;
  padding: 10px;
  border-top: 0;
  color: #333;
  .specification {
    border-left: 1px solid #dddddd;
    border-right: 1px solid #dddddd;
    border-top: 1px solid #dddddd;
    margin-bottom: 25px;
    .g_type {
      padding-left: 80px;
      border-bottom: 1px solid #ddd;
      span {
        margin: 10px 0;
        display: inline-block;
      }
      select {
        margin: 10px 0;
        width: 288px;
        height: 32px;
        border: 1px solid #cccccc;
      }
    }
    .goods_spec {
      border-bottom: 1px solid #ddd;
      padding: 10px 0;
      padding-left: 80px;
      font-weight: bold;
    }
    .g_size {
      padding-left: 100px;
      border-bottom: 1px solid #ddd;
      overflow: hidden;
      span {
        margin: 18px 0;
      }
      ul {
        margin: 12px 0;
        li {
          padding: 0 5px;
          height: 32px;
          text-align: center;
          line-height: 32px;
          background-color: #ddd;
          color: #333;
          display: block;
          margin-right: 25px;
          margin-top: 10px;
          float: left;
          border-radius: 4px;
          font-size: 14px;
          cursor: pointer;
        }
        li.color {
          background-color: #f7bc0a;
          color: #fff;
        }
      }
    }
  }
  .g_editss {
    margin-bottom: 25px;
  }
  .tijiao {
    height: 32px;
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
  .edit_btn {
    display: inline-block;
    margin: 1px;
    line-height: 50px;
    padding: 1px;
  }
  .edit_input {
    vertical-align: middle;
    margin: 10px;
    line-height: 10px;
    border-radius: 5px;
    border: 1px solid #dbdbdb;
    height: 26px;
  }
}
</style>
