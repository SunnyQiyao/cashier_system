<!--Pos组件-->
<template>
    <div class="pos">
        <div>
            <el-row>
                <!--订单展示区-->
                <el-col :span="7" id="orderList">
                    <el-tabs>
                        <el-tab-pane label="结账">
                            <el-table :data="tableData" style="width: 100%" border>
                                <el-table-column prop="goodsName" label="商品名称" width=""></el-table-column>
                                <el-table-column prop="price" label="金额" width=""></el-table-column>
                                <el-table-column prop="count" label="数量"></el-table-column>
                                <el-table-column fixed="right" label="操作">
                                    <template slot-scope="scope">
                                        <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                                        <el-button type="text" size="small" @click="minusOrderList(scope)">减少</el-button>
                                        <el-button type="text" size="small" @click="delSingleOrder(scope.row)">删除</el-button>
                                    </template>
                                </el-table-column>
                            </el-table>
                            <div class="countMoney">
                                <span>总金额：{{totalMoney}}元</span>
                                <span>总数量：{{totalCount}}</span>
                            </div>
                            <div class="bottomBtn">
                                <el-button type="danger" size="small" @click="delAllOrders">删除</el-button>
                                <el-button type="success" size="small" @click="squareAccounts">结账</el-button>
                            </div>
                        </el-tab-pane>
                        <el-tab-pane label="挂单">挂单</el-tab-pane>
                        <el-tab-pane label="外卖">外卖</el-tab-pane>
                    </el-tabs>
                </el-col>
                <!--商品选择区-->
                <el-col :span="17" id="chooseGoods">
                    <!--爆款商品-->
                    <div class="hotGoods">
                        <p class="allGoods">爆款商品</p>
                        <ul class="clearfix">
                            <li v-for="(good,index) in oftenGoods" :key="index" @click="addOrderList(good)">
                                <span>{{good.goodsName}}</span>
                                <span>￥{{good.price}}元</span>
                            </li>
                        </ul>
                    </div>
                    <!--分类商品-->
                    <div class="goodsType">
                       <el-tabs>
                            <el-tab-pane label="主食">
                                <ul class="clearfix">
                                    <li v-for="type0 in type0Goods" @click="addOrderList(type0)">
                                        <div class="foodImg">
                                            <img :src="type0.goodsImg" alt="">
                                        </div>
                                        <div class="detailFood">
                                            <span>{{type0.goodsName}}</span>
                                            <span>￥{{type0.price}}元</span>
                                        </div>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="小吃">
                                <ul class="clearfix">
                                    <li v-for="type1 in type1Goods" @click="addOrderList(type1)">
                                        <div class="foodImg">
                                            <img :src="type1.goodsImg" alt="">
                                        </div>
                                        <div class="detailFood">
                                            <span>{{type1.goodsName}}</span>
                                            <span>￥{{type1.price}}元</span>
                                        </div>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="饮料">
                                <ul class="clearfix">
                                    <li v-for="type2 in type2Goods" @click="addOrderList(type2)">
                                        <div class="foodImg">
                                            <img :src="type2.goodsImg" alt="">
                                        </div>
                                        <div class="detailFood">
                                            <span>{{type2.goodsName}}</span>
                                            <span>￥{{type2.price}}元</span>
                                        </div>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="套餐">
                                <ul class="clearfix">
                                    <li v-for="type3 in type3Goods" @click="addOrderList(type3)">
                                        <div class="foodImg">
                                            <img :src="type3.goodsImg" alt="">
                                        </div>
                                        <div class="detailFood">
                                            <span>{{type3.goodsName}}</span>
                                            <span>￥{{type3.price}}元</span>
                                        </div>
                                    </li>
                                </ul>
                            </el-tab-pane>
                        </el-tabs>
                    </div>
                </el-col>
            </el-row>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    export default{
        name:'Pos',
        data(){
            return{
                // 订单数据
                tableData:[],
                // 爆款商品
                oftenGoods:[],
                // 分类商品
                type0Goods:[],
                type1Goods:[],
                type2Goods:[],
                type3Goods:[],
                totalCount:0,
                totalMoney:0  //Vue data里面的数据。都可以直接用this 使用，因为this指向的就是Vue
            }
        },
        created(){
            // 获取爆款商品数据
            axios({
                methods:'get',
                url:'http://jspang.com/DemoApi/oftenGoods.php'
            })
            .then((response=>{
                // console.log(response)
                this.oftenGoods=response.data;
            }))
            .catch(err=>{
                console.log(err)
                alert('网络有误，访问失败')
            })

            // 获取分类商品数据
            axios.get('http://jspang.com/DemoApi/typeGoods.php')
            .then(response=>{
                // console.log(response)
                this.type0Goods=response.data[0],
                this.type1Goods=response.data[1],
                this.type2Goods=response.data[2],
                this.type3Goods=response.data[3]
            })
            .catch(err=>{
                console.log(err)
                alert('访问失败')
            })
        },
        mounted(){
            // 由于使用了element组件，会产生虚拟DOM，无法设置高度为100%，利用javascript自行设置
            var orderHeight=document.body.clientHeight;
            document.getElementById('orderList').style.height=orderHeight+'px'
            document.getElementById('chooseGoods').style.height=orderHeight+'px'
        },
        methods:{
            // 1.增加商品订单
            addOrderList(choosedgoods){ 
               this.totalCount=0; //this指向Vue
               this.totalMoney=0;
               let isExist=false;
               // 先判断订单列表中有没有这个商品    
               for(let i=0;i<this.tableData.length;i++){
                   if(this.tableData[i].goodsId==choosedgoods.goodsId){
                       isExist=true
                   }
               } 
               // 如果有的话就将订单列表的数量增加
               if(isExist){
                   let arr=this.tableData.filter(function(mygoods){
                       return mygoods.goodsId==choosedgoods.goodsId
                   })
                   arr[0].count++
                //    console.log(arr)
               }else{
                    // 如果没有就将商品添加到订单列表
                    let newGoods={goodsId:choosedgoods.goodsId,goodsName:choosedgoods.goodsName,price:choosedgoods.price,count:1}
                    this.tableData.push(newGoods)
               }  
               // 计算总数量和总金额
               this.getAllCountMoney()
            },
            // 2.减少商品订单
            minusOrderList(minusgood){
                let totalCount=0;
                let totalMoney=0;
                // console.log(minusgood)
                // console.log(this.tableData[minusgood.$index])
                if(this.tableData[minusgood.$index].count>1){
                    this.tableData[minusgood.$index].count--
                }else{
                    this.tableData.splice(minusgood.$index,1)
                }
                this.getAllCountMoney()
            },
            // 3.删除订单里的单个商品
            delSingleOrder(delGoods){
                console.log(delGoods)
                // this.tableData=this.tableData.filter(someGood=>someGood.goodsId!=delGoods.goodsId)
                this.tableData.splice(delGoods,1)
                this.getAllCountMoney()
            },
            // 4.删除全部订单商品
            delAllOrders(){
                this.tableData=[];
                this.totalCount=0;
                this.totalMoney=0
            },
            // 封装计算总数量和总金额的方法，以便在添加和删除单个订单时复用
            getAllCountMoney(){
                this.totalCount=0;
                this.totalMoney=0; 
                if(this.tableData){
                    var _this=this;
                    this.tableData.forEach(function(item){
                        _this.totalCount+=item.count;
                        _this.totalMoney+=item.count*item.price
                    })
                }
            },
            // 模拟结账
            squareAccounts(){
                if(this.totalCount!=0){
                    this.tableData=[];
                    this.totalCount=0;
                    this.totalMoney=0;
                    this.$message({
                        message:'恭喜结账成功，欢迎下次光临',
                        type:'success'
                    })
                }else{
                    this.$message({
                        message:'暂无商品需要结账，请核实',
                        type:'warning'
                    })
                }
            }
        }
    }
</script>

<style scoped>
    .clearfix::before,
    .clearfix::after{
        content:'';
        display:block;
        height: 0;
        line-height: 0;
        visibility:hidden;
        clear:both;
    }
    li{
        list-style:none;
    }
    .pos{
        text-align:center;
    }
    .countMoney{
        padding:20px 0;
        border-bottom: 1px solid #ebeef5;
    }
    .countMoney span:first-child{
        margin-right: 50px;
    }
    .bottomBtn{
        margin:20px 0;
    }
    #chooseGoods{
        background-color:#edf4f7;
    }
    .allGoods{
        width: 95%;
        text-align:left;
        padding-left:5%;
        height: 20px;
        line-height: 20px;
    }
    .hotGoods li{
        float:left;
        width:20%;
        margin-right: 3%;
        padding: 10px 5px;
        border:1px solid #E5E9F2;
        background-color:#fff;
        margin-bottom: 15px;
        cursor:pointer;
    }
    .hotGoods li span:nth-child(2){
        color:#58B7FF;
    }
    .goodsType{
        width: 95%;
        margin-left:5%;
        margin-top:50px;
    }
    .goodsType ul{
        width: 100%;
        padding-left:0;
    }
    .goodsType li{
        float:left;
        width:20%;
        margin-right:3%;
        margin-bottom: 15px;
    }
    .detailFood span{
        display:block;
    }
    .foodImg{
        width: 50%;
        float:left;
    }
    .foodImg img{
        width: 100%;
    }
</style>