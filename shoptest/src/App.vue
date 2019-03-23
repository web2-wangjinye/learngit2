<template>
  <div id="app" class="clearfix">
    <!-- <img src="./assets/logo.png"> -->
    <leftNav></leftNav>
    <div class="main">
      <router-view/>
    </div>
  </div>
</template>

<script>
import leftNav from '@/components/common/leftNav'
export default {
  name: 'app',
  components:{
    leftNav
  }
}
</script>

<style>
#app {
  font-family: 'Microsoft YaHei','Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
   height:100%;
   border:1px solid red;
}
 
.main{
  float:left;
  width:94%; 
  background-color: #EFF2F7;
  height:100%;
  overflow: auto;
  border:1px solid red;
}
.clearfix:after{
content:"";
display:block;
clear:both
}
</style>
