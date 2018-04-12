# -
图片懒加载技术

前端实现图片懒加载(lazyload)的两种方式


思路：
将页面里所有img属性src属性用data-xx代替，当页面滚动直至此图片出现在可视区域时，用js取到该图片的data-xx的值赋给src。

 

关于各种宽高：

页可见区域宽： document.body.clientWidth;
网页可见区域高： document.body.clientHeight;
网页可见区域宽： document.body.offsetWidth (包括边线的宽);
网页可见区域高： document.body.offsetHeight (包括边线的宽);
网页正文全文宽： document.body.scrollWidth;
网页正文全文高： document.body.scrollHeight;
网页被卷去的高： document.body.scrollTop;
网页被卷去的左： document.body.scrollLeft;
网页正文部分上： window.screenTop;
网页正文部分左： window.screenLeft;
屏幕分辨率的高： window.screen.height;
屏幕分辨率的宽： window.screen.width;
屏幕可用工作区高度： window.screen.availHeight;

 

示例：
jqueryLazyload方式
下载地址：https://github.com/helijun/helijun/blob/master/plugin/lazyLoad/jquery.lazyload.js

<section class="module-section" id="container">
     <img class="lazy-load" data-original="../static/img/loveLetter/teacher/teacher1.jpg" width="640" height="480" alt="测试懒加载图片"/>
</section>
 

require.config({
    baseUrl : "/static",
    paths: {
        jquery:'component/jquery/jquery-3.1.0.min'
        jqueryLazyload: 'component/lazyLoad/jquery.lazyload',//图片懒加载
    },
    shim: {
        jqueryLazyload: {
            deps: ['jquery'],
            exports: '$'
        }
    }
});

require(
    [
        'jquery',
        'jqueryLazyload'
    ], 
    function($){
        $(document).ready(function() {     
            $("img.lazy-load").lazyload({ 
　　　　　　　　　　effect : "fadeIn", //渐现，show(直接显示),fadeIn(淡入),slideDown(下拉)
　　　　　　　　　　threshold : 180, //预加载，在图片距离屏幕180px时提前载入
　　　　　　　　　　event: 'click',  // 事件触发时才加载，click(点击),mouseover(鼠标划过),sporty(运动的),默认为scroll（滑动）
　　　　　　　　　　container: $("#container"), // 指定对某容器中的图片实现效果
　　　　　　　　　　failure_limit：2 //加载2张可见区域外的图片,lazyload默认在找到第一张不在可见区域里的图片时则不再继续加载,但当HTML容器混乱的时候可能出现可见区域内图片并没加载出来的情况
　　　　　　　　}); 
　　　　　　});
　　});

 

为了代码可读性，属性值我都写好了注释。值得注意的是预制图片属性为data-original，并且最好是给予初始高宽占位，以免影响布局，当然这里为了演示我是写死的640x480，如果是响应式页面，高宽需要动态计算。

dome演示地址：http://h5.sztoda.cn/test/testLazyLoad

 

 

echo.js方式
在前面“前端知识的一些总结”的博文中，介绍了一款非常简单实用轻量级的图片延时加载插件echo.js，如果你的项目中没有依赖jquery，那么这将是个不错的选择，50行代码，压缩后才1k。当然你完全可以集成到自己项目中去！

下载地址：https://github.com/helijun/helijun/tree/master/plugin/echo

 

<style>
　　.demo img { 
　　　　width: 736px; 
　　　　height: 490px; 
　　　　background: url(images/loading.gif) 50% no-repeat;}
</style>

<div class="demo">
    <img class="lazy" src="images/blank.gif" data-echo="images/big-1.jpg">
</div>

<script src="js/echo.min.js"></script>

<script>

Echo.init({
    offset: 0,//离可视区域多少像素的图片可以被加载
　　 throttle: 0 //图片延时多少毫秒加载
}); 

</script>

说明：blank.gif是一张背景图片，包含在插件里了。图片的宽高必须设定，当然，可以使用外部样式对多张图片统一控制大小。data-echo指向的是真正的图片地址。
