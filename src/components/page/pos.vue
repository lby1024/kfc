<style>
.pos {
  height: 100%;
}
.pos-100 {
  height: 100%;
}
.el-col {
  height: 100%;
}
.pos-order {
  background-color: #f1f1f1;
  border-right: 3px solid #b7b5b5;
  padding: 12px;
}
.pos-goods {
  background-color: #f1f1f1;
}
.el-tabs__item {
  font-size: 24px;
}
.div-btn {
  text-align: center;
  margin-top: 15px;
}
.often-goods-container{
    padding: 15px;
    height: 35%;
    background-color: #ececec;
}
.often-goods-item{
    height: 60px;
    text-align: center;
}
.often-goods-title{
    height: 5%;
    padding-top: 12px;
    background-color: skyblue;
    text-align: center;
    color: white;
    font-weight: bold;
}
.often-goods-price{
    color: red;
    padding-left: 9px;
}
.pos-menu{
    height: 60%;
    background-color:#fff;
    padding-top: 9px;
}
.pos-menu-item{
    padding: 12px;
    height: 270px;
}
.pos-menu-img{
    width: 100%;
    height: 150px;
}

</style>

<template>
    <div class="pos">
        <el-row class="pos-100">
            <!-- right=============================================================================== -->
            <el-col :span='6' class="pos-order">
                <!-- 表单部分 -->
                <el-tabs v-model="activeName" @tab-click="handleClick">
                    <el-tab-pane label="点餐" name="first">
                        <el-table :data="tableData" border width='100%'>
                            <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                            <el-table-column prop="count" label="量" width="50"></el-table-column>
                            <el-table-column prop="price" label="单价" width="70"></el-table-column>
                            <el-table-column label="操作" width="100" fixed="right">
                                <template slot-scope="scope">
                                    <el-button type="text" size="small" @click="add(scope.row)">添加</el-button>
                                    <el-button type="text" size="small" @click="del_goods(scope.row)">删除</el-button>
                                </template>   
                            </el-table-column>
                        </el-table>
                    </el-tab-pane>
                    <el-tab-pane label="挂单" name="second">挂单</el-tab-pane>
                    <el-tab-pane label="外卖" name="third">外卖</el-tab-pane>
                </el-tabs>
                <!-- 汇总部分 -->
                <h3 class="total">
                    <span>总价 : {{total_price}}</span>
                </h3>
                <!-- 按钮部分 -->
                <div class="div-btn">
                    <el-button type="warning">挂单</el-button>
                    <el-button type="danger" @click="del_allgoods">取消</el-button>
                    <el-button type="success" @click="checkout">结账</el-button>
                </div>
            </el-col>
            <!-- left=============================================================================== -->

            <el-col :span="18" class="pos-goods">
                <!-- 热门商品 -->
                <el-row class="often-goods-title">
                    <el-col :span="24">热门商品</el-col>
                </el-row>
                <el-row class="often-goods-container">
                    <el-col :span="6" v-for="(item,index) in oftenGoods" :key="index" class="often-goods-item">
                        <el-button @click.native="add(item)">{{item.goodsName}}<span class="often-goods-price">¥{{item.price}}元</span></el-button>
                    </el-col>
                </el-row>
                <!-- 菜单 -->
                <el-row class="pos-menu">
                    <el-tabs v-model="activeName_right" @tab-click="handleClick">
                        <el-tab-pane label="汉堡" name="first">
                            <el-row>
                                <el-col :span="4" v-for="(o, index) in type0Goods0" :key="index" class="pos-menu-item">
                                    <el-card :body-style="{ padding: '0px' }" class="pos-memu-card" @click.native="add(o)">
                                        <img :src="o.goodsImg" class="pos-menu-img">
                                        <div style="padding: 14px;">
                                            <span>{{o.goodsName}}</span>
                                            <span>¥{{o.price}}元</span>
                                        </div>
                                    </el-card>
                                </el-col>
                            </el-row>
                        </el-tab-pane>
                        <el-tab-pane label="小吃" name="second">
                            <el-row>
                                <el-col :span="4" v-for="(o, index) in type0Goods1" :key="index" class="pos-menu-item">
                                    <el-card :body-style="{ padding: '0px' }" class="pos-memu-card" @click.native="add(o)">
                                        <img :src="o.goodsImg" class="pos-menu-img">
                                        <div style="padding: 14px;">
                                            <span>{{o.goodsName}}</span>
                                            <span>¥{{o.price}}元</span>
                                        </div>
                                    </el-card>
                                </el-col>
                            </el-row>
                        </el-tab-pane>
                        <el-tab-pane label="饮料" name="third">
                            <el-row>
                                <el-col :span="4" v-for="(o, index) in type0Goods2" :key="index" class="pos-menu-item">
                                    <el-card :body-style="{ padding: '0px' }" class="pos-memu-card" @click.native="add(o)">
                                        <img :src="o.goodsImg" class="pos-menu-img">
                                        <div style="padding: 14px;">
                                            <span>{{o.goodsName}}</span>
                                            <span>¥{{o.price}}元</span>
                                        </div>
                                    </el-card>
                                </el-col>
                            </el-row>
                        </el-tab-pane>
                        <el-tab-pane label="套餐" name="fourth">
                            <el-row>
                                <el-col :span="4" v-for="(o, index) in type0Goods3" :key="index" class="pos-menu-item">
                                    <el-card :body-style="{ padding: '0px' }" class="pos-memu-card" @click.native="add(o)">
                                        <img :src="o.goodsImg" class="pos-menu-img">
                                        <div style="padding: 14px;">
                                            <span>{{o.goodsName}}</span>
                                            <span>¥{{o.price}}元</span>
                                        </div>
                                    </el-card>
                                </el-col>
                            </el-row>
                        </el-tab-pane>
                    </el-tabs>
                </el-row>
            </el-col>
            
        </el-row>
    </div>
</template>

<script>
// import axios from 'axios'
import store from '../vuex/store'
import {mapState} from 'vuex'

export default {
    name: "pos",
    store,
    data() {
        return {
            activeName: "first",
            activeName_right: 'first',
            tableData: [],
            type0Goods0:[],
            type0Goods1:[],
            type0Goods2:[],
            type0Goods3:[],
            total_price: 0,
            total_count: 0
        };
    },
    methods: {
        handleClick(tab, event) {
            console.log(tab, event);
        },
        // 1 : 查看list里面商品是否已经存在
        // 2 : 如果存在这个商品的count值加一
        // --> 如果不存在将将商品添加到列表
        add(goods){
            let is_exist = false
            // 1 : 查看list里面商品是否已经存在
            for(let item of this.tableData){
                if(item.goodsId==goods.goodsId){
                    is_exist = true
                }
            }
            // 2 : 如果存在这个商品的count值加一
            if(is_exist){
                let arr = this.tableData.filter(item => item.goodsId==goods.goodsId)
                arr[0].count++
            // --> 如果不存在将将商品添加到列表
            }else{
                let new_goods = {goodsId: goods.goodsId , goodsName: goods.goodsName , price: goods.price , count: 1}
                this.tableData.push(new_goods)
            }

            this.get_all_money()
        },
        get_all_money(){
            // 从新计算总价需要清零
            this.total_price = 0
            // 依次计算累加
            for (let item of this.tableData){
                // row1_count * row1_price + row2_count * row2_price + ......
                this.total_price += item.count*item.price
            }
        },
        del_goods(goods){
            this.tableData = this.tableData.filter(item=> item.goodsId != goods.goodsId)
            this.get_all_money()
        },
        del_allgoods(){
            this.tableData = []
            this.total_price = 0
        },
        checkout(){
            // 1 : 提交数据
            // 2 : 接收返回值
            // 3 : 如果成功，清空现有构造器里的tableData，totalMoney，totalCount数据。
            // 4 : 进行用户的友好提示


            // 如果总价不为空,即是订单里没有商品
            if(this.total_price != 0){
                // 清空订单列表
                this.tableData = []
                // 总价归零
                this.total_price = 0
                // 弹出提示信息
                this.$message({
                    message: '结账成功，感谢你又为店里出了一份力!',
                    type: 'success'
                })
            }else{
                this.$message.error('不能空结。老板了解你急切的心情！')
            }
        }
    }
    ,
    computed: {
        ...mapState(['oftenGoods', 'typeGoods'])
    },
    created(){
        this.type0Goods0 = this.typeGoods[0]
        this.type0Goods1 = this.typeGoods[1]
        this.type0Goods2 = this.typeGoods[2]
        this.type0Goods3 = this.typeGoods[3]
    }
};
</script>