# 輪播圖

用戶登錄到首頁之後， 第一個看到的內容，就是輪播圖。 (見前面的原型圖)

所以，我們接下來，要爲首頁增加它。  

## 1. 增加路由

路由文件(src/router/index.js)，對應位置，增加： 

```
import Index from '@/views/shops/index'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'Home',
      component: Index
    }
  ]
```

## 2. 增加對應頁面

src/views/shops/index.vue

```
<template>
  <div class="background">
    <div class="home">
      <div class="m_layout">
        <!-- 輪播圖-->
        <HomeBannerView></HomeBannerView>
        <!--導航-->
        <HomeNavView></HomeNavView>
        
        <span class="divider" style="height: 4px;"></span>
        <div class="product_top">
          <div class="product_left">
            <div>商品列表</div>
          </div>
        </div>
        <span class="divider" style="height: 2px;"></span>
        <!-- 特產商品 -->
        <SpecialMarket :id="good.id" :name="good.name" :description="good.description" :image_url="good.image_url" v-for="good in goods"></SpecialMarket>
      </div>
    </div>
    <NavBottomView :is_shops_index="is_shops_index"></NavBottomView>
  </div>
</template>
<script>
    // 輪播圖需要的js文件
    import {bindEvent,scrollPic} from '../../libs/index.js'
    // 輪播圖需要的前臺文件
    import HomeBannerView from '../../components/HomeBanner.vue';
    // 商品分類
    import HomeNavView from '../../components/HomeNav.vue';

    // 特產，商品的列表
    import SpecialMarket from '../../components/SpecialMarket.vue';

    // 底部4個TAB， 下一節會講到
    import NavBottomView from '../../components/NavBottom.vue';

    export default{
      data () {
        return {
          goods: [],
          is_shops_index: true,
        }
      },
       components:{
        HomeHeaderView,
        HomeBannerView,
        HomeNavView,
        HomeMainView,
        SpecialMarket,
        NavBottomView
       },
       mounted () {
        //bindEvent();
        scrollPic();
        this.loadPage ();
       },
       computed:  {
       },
       methods: {
         loadPage () {
           this.$http.get(this.$configs.api + 'goods/get_goods').then((response)=>{
             console.info(response.body)
             this.goods= response.body.goods
           },(error) => {
             console.error(error)
           });
         },
       }
    }
</script>

```

上面的代碼中， 

- 先是讀取了一個API
- 渲染數據
- 實現首頁的輪播圖



## 3. 增加輪播圖

我們做開發的核心方法論： 不要造輪子。 

一定要造輪子的話， 先搜索一下是否已經有了現成的輪子。 

輪播圖是最最常見的組件， 所以一定會有別人寫好的第三方包， 我們拿過來用就好。 

經過一番考察， 我們發現這個項目不錯： 

所以， 直接拿過來用。  


3.1 輪播圖的組件

我們需要在src/views/shops/index.vue中，增加： 

```
import {bindEvent,scrollPic} from '../../libs/index.js'
```

然後， 增加對應的文件（libs/index.js)

```

function $id(id) {
    return document.getElementById(id);
}

function bindEvent() {
    var sea = $id("my_search");
    /*banner對象*/
    var banner = $id("my_banner");
    /*高度*/
    var height = banner.offsetHeight;
    window.onscroll = function() {
        var top = document.body.scrollTop;
        /*當滾動高度大於banner的高度時候顏色不變*/
        if (top > height) {
            sea.style.background = "rgba(201,21,35,0.85)";
        } else {
            var op = top / height * 0.85;
            sea.style.background = "rgba(201,21,35," + op + ")";
        }
    };
}

function scrollPic() {
    var imgBox = document.getElementsByClassName("banner_box")[0];
    var width = $id("my_banner").offsetWidth;
    var pointBox = document.getElementsByClassName("point_box")[0];
    var ols = pointBox.children;
    var indexx = 1;
    var timer = null;
    var moveX = 0;
    var endX = 0;
    var startX = 0;
    var square = 0;

    function addTransition() {
        imgBox.style.transition = "all .3s ease 0s";
        imgBox.style.webkitTransition = "all .3s ease 0s";
    }

    function removeTransition() {
        imgBox.style.transition = "none";
        imgBox.style.webkitTransition = "none";
    }

    function setTransfrom(t) {
        imgBox.style.transform = 'translateX(' + t + 'px)';
        imgBox.style.webkitTransform = 'translateX(' + t + 'px)';
    }

    // 開始動畫部分
    pointBox.children[0].className = "now";
    for (var i = 0; i < ols.length; i++) {
        ols[i].index = i; // 獲得當前第幾個小li 的索引號
        ols[i].onmouseover = function() {
            // 所有的都要清空
            for (var j = 0; j < ols.length; j++) {
                ols[j].className = "";
            }
            this.className = "now";
            setTransfrom(-indexx * width);
            square = indexx; 
        }
    }
    timer = setInterval(function() {
        indexx++;
        addTransition();
        setTransfrom(-indexx * width);
        // 小方塊
        square++;
        if (square > ols.length - 1) {
            square = 0;
        }
        // 先清除所有的
        for (var i = 0; i < ols.length; i++) 
        {
            ols[i].className = "";
        }
        // 留下當前的
        ols[square].className = "now"; 
    }, 3000);

    imgBox.addEventListener('transitionEnd', function() {
        if (indexx >= 9) {
            indexx = 1;
        } else if (indexx <= 0) {
            indexx = 8;
        }
        removeTransition();
        setTransfrom(-indexx * width);
    }, false);

    imgBox.addEventListener('webkitTransitionEnd', function() {
        if (indexx >= 9) {
            indexx = 1;
        } else if (indexx <= 0) {
            indexx = 8;
        }
        removeTransition();
        setTransfrom(-indexx * width);
    }, false);

    /**
     * 觸摸事件開始
     */
    imgBox.addEventListener("touchstart", function(e) {
        console.log("開始");
        var event = e || window.event;
        //記錄開始滑動的位置
        startX = event.touches[0].clientX;
    }, false);

    /**
     * 觸摸滑動事件
     */
    imgBox.addEventListener("touchmove", function(e) {
        console.log("move");
        var event = e || window.event;
        event.preventDefault();

        //清除定時器
        clearInterval(timer);
        //記錄結束位置
        endX = event.touches[0].clientX;
        //記錄移動的位置
        moveX = startX - endX;
        removeTransition();
        setTransfrom(-indexx * width - moveX);
    }, false);

    /**
     * 觸摸結束事件
     */
    imgBox.addEventListener("touchend", function() {
        console.log("end");
        //如果移動的位置大於三分之一，並且是移動過的
        if (Math.abs(moveX) > (1 / 3 * width) && endX != 0) {
            //向左
            if (moveX > 0) {
                indexx++;
            } else {
                indexx--;
            }
            //改變位置
            setTransfrom(-indexx * width);
        }
        //回到原來的位置
        addTransition();
        setTransfrom(-indexx * width);
        //初始化
        startX = 0;
        endX = 0;

        clearInterval(timer);
        timer = setInterval(function() {
            indexx++;
            square++;
            if (square > ols.length - 1) {
                square = 0;
            }
            // 先清除所有的
            for (var i = 0; i < ols.length; i++) 
            {
                ols[i].className = "";
            }
            // 留下當前的
            ols[square].className = "now"; 
            addTransition();
            setTransfrom(-indexx * width);

        // 每3秒鐘輪播圖變化一次。
        }, 3000);
    }, false);
};

module.exports = {
    bindEvent,
    scrollPic
}

```


3.2 輪播圖的視圖層

```
<!-- 輪播圖-->
<HomeBannerView></HomeBannerView>   
```

上面的代碼， 會直接生成一個輪播圖。


我們創建這個component, 增加： src/components/HomeBanner.vue, 

```
<template>
	<div class="home_ban">
		<div class="m_banner clearfix" id="my_banner">
	        <ul class="banner_box">
	            <!-- 更改這裏就可以替換輪播圖的圖片了 -->
	            <li><img src="http://siweitech.b0.upaiyun.com/image/silulegou/2FgHsjCz7qfpSQr0.jpeg" alt="" /></li>
	            <li><img src="http://siweitech.b0.upaiyun.com/image/silulegou/uJawxX6H3PBRcfMO.jpeg" alt="" /></li>
	        </ul>
	        <ul class="point_box" >
	            <li></li>
	            <li></li>
	        </ul>
        </div>
	</div>
</template>
```

這個component的內容非常簡單。 它只是一個輪播圖組件的View. 我們只需要更換上面代碼中的兩個圖片文件就好了。

## 4. 增加物品分類

```
<!--商品分類-->
<HomeNavView></HomeNavView>
```

上面的代碼， 會直接生成一個物品分類區域

我們創建這個component: 增加： src/components/HomeNav.vue

```
<template>
	<div class="home_n">
		<nav class="m_nav">
                <ul>
                    <li class="nav_item">
                        <a href="#" class="nav_item_link">
                            <img src="../assets/images/nav0.png" alt="">
                            <span>草原特色肉</span>
                        </a>
                    </li>
                    <li class="nav_item">
                        <a href="#" class="nav_item_link">
                            <img src="../assets/images/nav1.png" alt="">
                            <span>特色乾果</span>
                        </a>
                    </li>
                    <li class="nav_item">
                        <a href="#" class="nav_item_link">
                            <img src="../assets/images/nav9.png" alt="">
                            <span>特色瓜子</span>
                        </a>
                    </li>
                    <li class="nav_item">
                        <a href="#" class="nav_item_link">
                            <img src="../assets/images/nav8.png" alt="">
                            <span>特色大米</span>
                        </a>
                    </li>
                </ul>
            </nav>
	</div>
</template>

```

之所以這樣寫， 是考慮到前期的商品分類不多。 在下一個版本會改成讀取後臺的接口。 


## 看效果  

默認頁面效果如下：

![圖1](/images/real_project/slider_1.png)

三秒鐘之後， 輪播圖發生了滾動：

![圖2](/images/real_project/slider_2.png)

## 總結

我們使用了 現成的輪播圖組件。 可以看到， 非常簡單。 步驟是：

1. 複製對應組件的js 文件到src/lib下。
2. 複製對應組件的vue文件到src/components下
3. 在對應的vue文件中使用它們。

