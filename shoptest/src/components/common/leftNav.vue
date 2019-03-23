<template>
    <div class="leftnav">
<ul>
    <li>
        <i class="icon iconfont icon-kefu"></i>
        <div>店铺</div>
    </li>
    <li>
        <i class="icon iconfont icon-kefu"></i>
        <div>收银</div>
    </li>
    <li>
        <i class="icon iconfont icon-kefu"></i>
        <div>商品</div>
    </li>
    <li>
        <i class="icon iconfont icon-kefu"></i>
        <div>会员</div>
    </li>
    <li>
        <i class="icon iconfont icon-kefu"></i>
        <div>统计</div>
    </li>
    <li>
        <i class="icon iconfont icon-kefu"></i>
        <div>设置</div>
    </li>

</ul>
    </div>
</template>
<script>
export default {
  name: 'leftNav'
}
</script>
<style>
.leftnav{
    color: #fff;
    font-size: 10px;
    height:100%;
    background-color: #1d8ce0;
    float: left;
    width: 5%;
}
.iconfont{
    font-size: 24px;
}
.leftnav ul{
    padding: 0px;
    margin: 0px;
}
.leftnav li{
    list-style: none;
    text-align: center;
    border-bottom: 1px solid #20a0ff;
    padding:10px;
}
</style>
