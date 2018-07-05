<!--左侧导航组件-->
<template>
    <div class="leftNav">
        <ul>
            <li>
                 <i class="icon iconfont icon-shouyin"></i>
                 <span>收银</span>
            </li>
            <li>
                 <i class="icon iconfont icon-msnui-dianpu"></i>
                 <span>店铺</span>
            </li>
            <li>
                 <i class="icon iconfont icon-shangpin-"></i>
                 <span>商品</span>
            </li>
            <li>
                 <i class="icon iconfont icon-huiyuan"></i>
                 <span>会员</span>
            </li>
            <li>
                 <i class="icon iconfont icon-tongji"></i>
                 <span>统计</span>
            </li>
        </ul>
    </div>
</template>

<script>
    export default{
        name:'leftNav',
        data(){
            return{

            }
        }
    }
</script>

<style scoped>
    .leftNav{
        float:left;
        width: 6%;
        height: 100%;
        color: #fff;
        font-size: 10px;
        background-color: #1D8ce0;
    }
    .leftNav ul{
        padding: 0;
        margin: 0;
    }
    .leftNav li{
        list-style:none;
        text-align:center;
        border-bottom:1px solid #ccc;
        padding:10px 0;
        cursor:pointer;
    }
    .leftNav li i{
        font-size: 24px;
        display:block;
        margin-bottom:3px;
    }
</style>