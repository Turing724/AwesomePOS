<template>
  <div id="Pos" v-loading="loading">

    <el-row>
      <el-col :span="7" class="pos-order" id="orderList">
        <el-tabs type="card">
          <el-tab-pane label="点餐">
            <el-table border :data="tableData" style="width:100%">
              <el-table-column prop="goodsName" label="商品名称">

              </el-table-column>
              <el-table-column prop="count" label="数量" width="50">

              </el-table-column>
              <el-table-column prop="price" label="金额" width="60">

              </el-table-column>
              <el-table-column fixed="right" label="操作" width="100">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">
                    删除
                  </el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">
                    增加
                  </el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="total-box">
              <span>
                <small>金额：</small>
                {{totalMoney}}
              </span>
              <span>
                <small>数量：</small>
                {{totalCount}}
              </span>
            </div>
            <div class="div-btn">
              <el-button type="warning" size="small">
                挂单
              </el-button>
              <el-button type="danger" size="small" @click="delAllGoods">
                删除
              </el-button>
              <el-button type="success" size="small" @click="checkOut">
                结账
              </el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">
            挂单
          </el-tab-pane>
          <el-tab-pane label="外卖">
            外卖
          </el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="17">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-order-list">
            <ul>
              <li v-for="(item,index) in oftenGoods" :key="index" @click="addOrderList(item)">
                <span>{{item.goodsName}}</span>
                <span class="o-price">￥{{item.price}}</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class='cookList'>
                  <li v-for="item in type0Goods" :key="item.goodsId" @click="addOrderList(item)">
                    <span class="foodImg"><img :src="item.goodsImg" width="100%" height="80"></span>
                    <span class="foodName">{{item.goodsName}}</span>
                    <span class="foodPrice">￥{{item.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div>
                <ul class='cookList'>
                  <li v-for="item in type1Goods" :key="item.goodsId" @click="addOrderList(item)">
                    <span class="foodImg"><img :src="item.goodsImg" width="100%" height="80"></span>
                    <span class="foodName">{{item.goodsName}}</span>
                    <span class="foodPrice">￥{{item.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div>
                <ul class='cookList'>
                  <li v-for="item in type2Goods" :key="item.goodsId" @click="addOrderList(item)">
                    <span class="foodImg"><img :src="item.goodsImg" width="100%" height="80"></span>
                    <span class="foodName">{{item.goodsName}}</span>
                    <span class="foodPrice">￥{{item.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div>
                <ul class='cookList'>
                  <li v-for="item in type3Goods" :key="item.goodsId" @click="addOrderList(item)">
                    <span class="foodImg"><img :src="item.goodsImg" width="100%" height="80"></span>
                    <span class="foodName">{{item.goodsName}}</span>
                    <span class="foodPrice">￥{{item.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>

  </div>
</template>
<script>
import axios from 'axios';
export default {
  name: 'pos',

  data() {
    return {
      loading: false,
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalMoney: 0,
      totalCount: 0
    };
  },
  mounted() {
    let orderHeight = document.body.clientHeight;
    document.getElementById('orderList').style.height = orderHeight + 'px';
  },
  created() {
    this.getOftenGoodsData();
    this.getTypeGoodsData();
  },
  methods: {
    getOftenGoodsData() {
      this.loading = true;
      axios
        .get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
        .then(res => {
          this.oftenGoods = res.data;
          this.loading = false;
        })
        .catch(error => {
          console.log(error);
          this.loading = false;
        });
    },
    getTypeGoodsData() {
      this.loading = true;
      axios
        .get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods')
        .then(res => {
          this.type0Goods = res.data[0];
          this.type1Goods = res.data[1];
          this.type2Goods = res.data[2];
          this.type3Goods = res.data[3];
          this.loading = false;
        })
        .catch(error => {
          console.log(error);
          this.loading = false;
        });
    },
    addOrderList(goods) {
      this.totalMoney = 0;
      this.totalCount = 0;
      let isHave = false;
      // 判断是否存在
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId === goods.goodsId) {
          isHave = true;
        }
      }
      // 根据判断的值编写逻辑
      if (isHave) {
        // 改变列表中商品数量
        let arr = this.tableData.filter(x => x.goodsId === goods.goodsId);
        console.log(arr, '=====arr');
        arr[0].count++;
      } else {
        let newGoods = { goodsId: goods.goodsId, goodsName: goods.goodsName, price: goods.price, count: 1 };
        this.tableData.push(newGoods);
      }
      this.getAllMoney();
    },
    getAllMoney() {
      this.totalMoney = 0;
      this.totalCount = 0;
      if (this.tableData) {
        this.tableData.forEach(item => {
          this.totalCount += item.count;
          this.totalMoney = this.totalMoney + item.price * item.count;
        });
      }
    },

    delSingleGoods(goods) {
      this.tableData = this.tableData.filter(x => x.goodsId !== goods.goodsId);
      this.getAllMoney();
    },
    delAllGoods() {
      this.tableData = [];
      this.totalMoney = 0;
      this.totalCount = 0;
    },
    checkOut() {
      this.loading = true;
      if (this.totalCount != 0) {
        this.$message({
          message: '结账成功，感谢你又为店里出了一份力!',
          type: 'success'
        });
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
      } else {
        this.$message.error('不能空结。老板了解你急切的心情！');
      }
      this.loading = false;
    }
  }
};
</script>
<style>
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}
.div-btn {
  margin-top: 10px;
}
.often-goods {
}
.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
  text-align: left;
}
.often-order-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
  cursor: pointer;
}
.o-price {
  color: #58b7ff;
}
.goods-type {
  clear: both;
  padding-left: 20px;
}
.cookList li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auot;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
  cursor: pointer;
}
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
}
.foodName {
  font-size: 16px;
  width: 50%;
  text-align: left;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
.total-box {
  background-color: #fff;
  padding: 10px;
  border-bottom: 1px solid #d3dce6;
}
.total-box span {
  margin: 0 5px;
}
</style>
