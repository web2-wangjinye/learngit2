<template>
  <div class="pos">
    <el-row>
      <el-col :span='7' class="pos-order" id="orderlist">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tabledata" border style="width:100%;">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="数量"></el-table-column>
              <el-table-column prop="price" label="金额"></el-table-column>
              <el-table-column  label="操作" fixed="right">
                <template scope="scope">
                  <el-button type="text" size="small" @click="delStringGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="totalDiv">
              <small>数量:</small>{{totalcount}} <small>金额:</small>{{totalmoney}}
            </div>
            <div class="div-btn">
            <el-button type="warning">挂单</el-button>
            <el-button type="danger" @click="delAllGoods">删除</el-button>
            <el-button type="success" @click="checout">结账</el-button>
          </div>
          </el-tab-pane>
          
          <el-tab-pane label="挂单">挂单</el-tab-pane>
          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>

      </el-col>

      <el-col :span='17'>
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="goods in oftengoods" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">{{goods.price}}</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class='cookList'>
                    <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{goods.goodsName}}</span>
                        <span class="foodPrice">{{goods.price}}</span>
                    </li>
                </ul>
              </div>
            </el-tab-pane>
         
            <el-tab-pane label="小吃">
               <div>
                <ul class='cookList'>
                    <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{goods.goodsName}}</span>
                        <span class="foodPrice">{{goods.price}}</span>
                    </li>
                </ul>
              </div>
            </el-tab-pane>
          
            <el-tab-pane label="水果">
               <div>
                <ul class='cookList'>
                    <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{goods.goodsName}}</span>
                        <span class="foodPrice">{{goods.price}}</span>
                    </li>
                </ul>
              </div>
            </el-tab-pane>
          
            <el-tab-pane label="饮料">
               <div>
                <ul class='cookList'>
                    <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{goods.goodsName}}</span>
                        <span class="foodPrice">{{goods.price}}</span>
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
import axios from "axios";
export default {
  name: 'pos',
  data(){
    return {
      tabledata:[],
        oftengoods:[],
        type0Goods:[],
      type1Goods:[],
      type2Goods:[],
      type3Goods:[],
      totalmoney:"",
      totalcount:""
    }
  },
  created:function(){
    axios.get('http://jspang.com/DemoApi/oftenGoods.php')
    .then(response=>{
this.oftengoods = response.data;
    })
    .catch(error=>{
      console.log(error);
      alert("网络错误不能访问");
    })

    axios.get('http://jspang.com/DemoApi/typeGoods.php')
    .then(response=>{
console.log(response);
 this.type0Goods = response.data[0];
 this.type1Goods = response.data[1];
 this.type2Goods = response.data[2];
 this.type3Goods = response.data[3];
    })
    .catch(error=>{
      console.log(error);
      alert("网络错误不能访问");
    })
  },
  mounted:function name(params) {
    //mounted:当所有虚拟dom加载完成之后；template是虚拟dom
    var orderHeight = document.body.clientHeight;
    console.log(orderHeight);
    document.getElementById('orderlist').style.height = orderHeight+'px'
  },
  methods:{
    addOrderList(goods){
      this.totalmoney=0;
      this.totalcount = 0;
      //商品是否已经存在订单列表中
      let isHave = false;
      for(let i =0;i<this.tabledata.length;i++){
        if(this.tabledata[i].goodsId==goods.goodsId){
          isHave = true;
        }
      }
      //根据判断的值编写业务逻辑
      if(isHave){
        //改变列表中的商品数量
        let arr = this.tabledata.filter(o=>o.goodsId==goods.goodsId);
        arr[0].count++;
      }else{
        let newGoods = {
          goodsId:goods.goodsId,
          goodsName:goods.goodsName,
          price:goods.price,
          count:1
        }
        this.tabledata.push(newGoods);
      }
      //计算总金额和数量
     this.getALLMoney();
    },
    //删除单个商品
    delStringGoods(goods){
      this.tabledata = this.tabledata.filter(o=>o.goodsId!=goods.goodsId);
this.getALLMoney();
console.log(1);
    },
    delAllGoods(){
      this.totalcount=0;
      this.totalmoney=0;
      this.tabledata=[];
    },
    //模拟结账
    checout(){
      if(this.totalcount!=0){
         this.totalcount=0;
      this.totalmoney=0;
      this.tabledata=[];
      this.$message({
        message:"结账成功，感谢您为店里出来一份力",
        type:'success'
      })
      }else{
        this.$message.error('不能空结账')
      }
    },
    //汇总数量和金额
    getALLMoney(){
this.totalcount=0;
this.totalmoney = 0;
if(this.tabledata){
        //计算总金额和数量
      this.tabledata.forEach(element => {
        this.totalcount+=element.count;
        this.totalmoney = this.totalmoney+(element.price*element.count);
      });
}
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pos-order{
  background-color:#f9fafc;
  border-right:1px solid #c0ccda; 
}
.div-btn{
  margin:4px auto;
  text-align: center;
}
 .title{
       height: 20px;
       border-bottom:1px solid #D3DCE6;
       background-color: #F9FAFC;
       padding:10px;
   }
   .often-goods-list ul li{
      list-style: none;
      float:left;
      border:1px solid #E5E9F2;
      padding:10px;
      margin:5px;
      background-color:#fff;
   }
  .o-price{
      color:#58B7FF; 
   }
   .goods-type{
     clear:both;
   }
   .cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
       cursor:pointer;
 
   }
   .cookList li span{
       
        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;
 
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
   .totalDiv{
     background-color:#fff;
     padding:10px;
     border-bottom:1px solid #333;
   }
</style>
