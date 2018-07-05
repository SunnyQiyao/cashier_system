<template>
  <div id="app">
    <!--左侧导航-->
    <leftNav></leftNav>
    
    <!--右侧内容区域1-->
    <div class="right_content">
        <router-view/>
    </div>
  </div>
</template>

<script>
import leftNav from '@/components/common/leftNav'
export default {
  name: 'App',
  components:{
    leftNav
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.right_content{
  width: 94%;
  height: 100%;
  padding-left:6%;
}
.el-tabs__nav-wrap{
    padding-left:15px!important;
}
</style>
