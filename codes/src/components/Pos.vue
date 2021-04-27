<template>
  <!-- 页面的主体部分 -->
  <div class="pos">
    <div>
      <el-row>
        <!-- 订单栏 -->
        <el-col :span="7" class="order-tab" id="order-list">
          <el-tabs id="order-tabs">
            <!-- 点餐部分 -->
            <el-tab-pane id="order-meal" label="点餐">
              <el-table :data="tableData" border style="width:100%">
                <el-table-column prop="goodsName" label="商品"></el-table-column>
                <el-table-column prop="count" label="数量" width="50"></el-table-column>
                <el-table-column prop="price" label="金额" width="70"></el-table-column>
                <el-table-column label="操作" width="100" fixed="right">
                  <template scope="scope">
                    <el-button type="text" size="small" @click="deleteSingleGoods(scope.row)">删除</el-button>
                    <!-- 注意：这里要使用scope：row才能操作数据tableData -->
                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div class="total">
                <div>
                  <small>总金额：</small>
                  {{totalMoney}}
                </div>
                <div>
                  <small>总数量：</small>
                  {{totalCount}}
                </div>
              </div>
              <div id="handle-options">
                <el-button type="warning" @click="registerOrder">挂单</el-button>
                <el-button type="danger" @click="deleteAllGoods">删除</el-button>
                <el-button type="success" @click="checkOut">结账</el-button>
              </div>
            </el-tab-pane>
            <!-- 挂单部分 -->
            <el-tab-pane label="挂单">挂单</el-tab-pane>
            <!-- 外卖部分 -->
            <el-tab-pane label="外卖">外卖</el-tab-pane>
          </el-tabs>
        </el-col>

        <!-- 商品栏 -->
        <el-col :span="17">
          <div class="often-goods">
            <!-- 热销商品部分 -->
            <div class="title">热销商品</div>
            <div class="often-goods-list">
              <ul>
                <li
                  class="hotItem"
                  v-for="(item,index) in hotGoods"
                  :key="index"
                  @click="addOrderList(item)"
                >
                  <span>{{item.goodsName}}</span>
                  <span class="o-price">￥{{item.price}}</span>
                </li>
              </ul>
            </div>
            <!-- 商品分类部分 -->
            <div class="goods-type">
              <el-tabs>
                <el-tab-pane id="hamburger" label="汉堡">
                  <ul class="cookList">
                    <li
                      class="typeItem"
                      v-for="(item,index) in type0Goods"
                      :key="index"
                      @click="addOrderList(item)"
                    >
                      <span class="foodImg">
                        <img :src="item.goodsImg" width="100%" />
                      </span>
                      <span class="foodName">{{item.goodsName}}</span>
                      <span class="foodPrice">￥{{item.price}}</span>
                    </li>
                  </ul>
                </el-tab-pane>
                <el-tab-pane label="小食">
                  <ul class="cookList">
                    <li v-for="(item,index) in type1Goods" :key="index" @click="addOrderList(item)">
                      <span class="foodImg">
                        <img :src="item.goodsImg" width="100%" />
                      </span>
                      <span class="foodName">{{item.goodsName}}</span>
                      <span class="foodPrice">￥{{item.price}}</span>
                    </li>
                  </ul>
                </el-tab-pane>
                <el-tab-pane label="饮料">
                  <ul class="cookList">
                    <li v-for="(item,index) in type2Goods" :key="index" @click="addOrderList(item)">
                      <span class="foodImg">
                        <img :src="item.goodsImg" width="100%" />
                      </span>
                      <span class="foodName">{{item.goodsName}}</span>
                      <span class="foodPrice">￥{{item.price}}</span>
                    </li>
                  </ul>
                </el-tab-pane>
                <el-tab-pane label="套餐">
                  <ul class="cookList">
                    <li v-for="(item,index) in type3Goods" :key="index" @click="addOrderList(item)">
                      <span class="foodImg">
                        <img :src="item.goodsImg" width="100%" />
                      </span>
                      <span class="foodName">{{item.goodsName}}</span>
                      <span class="foodPrice">￥{{item.price}}</span>
                    </li>
                  </ul>
                </el-tab-pane>
              </el-tabs>
            </div>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Pos",
  data() {
    return {
      //商品临时数据
      tableData: [],
      // 热销商品
      hotGoods: [],
      // 分类商品
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      // 总金额和数量
      totalCount: 0,
      totalMoney: 0,
    };
  },
  created() {
    //获取热销商品
    axios
      .get(
        "https://www.fastmock.site/mock/0bf6a5bae7eab8507e44b56191ddff36/vuepos/oftenGoods"
      )
      .then((res) => {
        this.hotGoods = res.data.oftenGoods;
      })
      .catch((error) => {
        alert("网络错误，访问异常");
        console.log(error);
      });
    //获取分类商品，图片访问不了
    axios
      .get(
        "https://www.fastmock.site/mock/0bf6a5bae7eab8507e44b56191ddff36/vuepos/typeGoods"
      )
      .then((res) => {
        this.type0Goods = res.data.data[0];
        this.type1Goods = res.data.data[1];
        this.type2Goods = res.data.data[2];
        this.type3Goods = res.data.data[3];
        console.log(res.data.data);
      })
      .catch((error) => {
        alert("网络错误，访问异常");
        console.log(error);
      });
  },

  mounted() {
    //让tab栏的高度和窗口可视区一致
    let orderHeight = document.body.clientHeight;
    document.querySelector("#order-list").style.height = orderHeight + "px";
  },
  methods: {
    //添加商品订单
    addOrderList(good) {
      this.totalCount = 0;
      this.totalMoney = 0;
      let isExist = false;
      //判断商品是否已经存在订单列表里
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId === good.goodsId) {
          //已经存在
          isExist = true;
        }
      }
      // 已经存在该商品
      if (isExist) {
        let arr = this.tableData.filter((el) => {
          //arr就是已经存在的商品
          return el.goodsId === good.goodsId;
        });
        arr[0].count++;
      } else {
        //不存在该商品，创建新对象（商品），加到数组tableData中
        let newGoods = {
          goodsId: good.goodsId,
          goodsName: good.goodsName,
          price: good.price,
          count: 1,
        };
        this.tableData.push(newGoods);
      }
      // 获得总金额和总数量
      this.getAllMoney();
    },

    //删除单个商品订单
    deleteSingleGoods(good) {
      // 把要删除的订单过滤掉，就达到了删除效果
      this.tableData = this.tableData.filter(
        (el) => el.goodsId !== good.goodsId
      );
      // 删除后重新计算总金额和总数
      this.getAllMoney();
    },

    //挂单
    registerOrder() {
      if (this.totalCount) {
        this.$message.success("挂单成功，感谢您的使用！");
      } else {
        this.$message.warning("你还未选择商品");
      }
    },

    //删除全部商品订单
    deleteAllGoods() {
      this.tableData = [];
      this.totalCount = 0;
      this.totalMoney = 0;
    },

    // 结账
    checkOut() {
      // 如果有订单就结账成功，没有订单就提示
      if (this.totalCount) {
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
        this.$message({
          message: "结账成功，感谢您的使用！",
          type: "success",
        });
      } else {
        this.$message.error("您还未选择商品！");
      }
    },

    //计算总金额和总数量（单独封装成一个函数）
    getAllMoney() {
      //先清零，再计算
      this.totalMoney = 0;
      this.totalCount = 0;
      if (this.tableData) {
        this.tableData.forEach((el) => {
          this.totalCount += el.count;
          this.totalMoney += el.price * el.count;
        });
      }
    },
  },
};
</script>

<style  scoped>
.pos {
  height: 100%;
}
.order-tab {
  background-color: #f9fafe;
  border-right: 1px solid lightgray;
  text-align: center;
}
#handle-options {
  margin-top: 10px;
}
.title {
  height: 40px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
  text-align: left;
}
.often-goods {
  overflow: hidden;
}
.often-goods-list ul {
  padding: 30px;
}
/* 热销商品 */
.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  border-radius: 8px;
  padding: 10px;
  margin: 5px;
  background-color: #fff;
}
.o-price,
.foodPrice {
  color: #ff8d58;
}
.goods-type {
  clear: both;
  margin-top: 130px;
}
.goods-type el-tab-pane {
  padding-left: 30px;
}
/* 分类商品 */
.cookList li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  border-radius: 8px;
  height: auot;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
}
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
}
.foodImg img {
  width: 88px;
  height: 64px;
}
.foodName {
  font-size: 16px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
.total {
  display: flex;
  margin: 10px 0 10px 0;
}
.total div {
  flex: 1;
}
.hotItem,
.typeItem {
  cursor: pointer;
}
#order-meal,
#hamburger {
  padding-left: 30px !important;
}
</style>