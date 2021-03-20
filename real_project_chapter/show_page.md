# 商品詳情頁

當用戶在商品列表頁中點擊時，就會跳轉到該頁面。

步驟如下：

## 1. 新增路由

向src/router/index.js中增加： 

```
import GoodsDetails from '@/views/shops/goods_details'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/shops/goods_details',
      name: 'GoodsDetails',
      component: GoodsDetails
    },
  ]
})
```    

## 2. 新增vue頁面

向 src/views/shops/goods_details 中增加：

```
<template>
  <div class="background">
    <div class="goods_detail" style="height: 100%;">
      <header class="top_bar">
        <a onclick="window.history.go(-1)" class="icon_back"></a>
        <h3 class="cartname">商品詳情</h3>
      </header>
      <div class="tast_list_bd" style="padding-top: 44px;">
        <main class="detail_box">

        <!-- 輪播圖 -->
        <div class="home_ban">
          <div class="m_banner clearfix" id="my_banner">
            <ul class="banner_box" >
              <div v-for="image in good_images">
                <li><img :src="image" alt="" style="height: 300px"/></li>
              </div>
            </ul>
            <ul class="point_box" >
              <li></li>
            </ul>
          </div>
        </div>

        <section class="product_info clearfix">
          <div class="product_left">
            <p class="p_name">{{good.name}}</p>
            <div class="product_pric">
              <span>￥</span>
              <span class="rel_price">{{good.price}}</span>
              <span></span>

              <span style='color: grey;
              text-decoration: line-through;
              font-size: 18px;
              margin-left: 14px;'>
                原價: ￥{{good.original_price}}
              </span>
            </div>
            <!--
            <div class="product_right">
              降價通知
            </div>
            -->
          </div>
        </section>

        <span class="divider" style="height: 2px;"></span>
        <div id="choose_number" style= "height: 40px; background-color: #fff;">
          <label style="font-size: 18px; float: left; margin-left: 10.5px; margin-top: 7.5px;">購買數量</label>
          <div style= "padding-top: 5px;">
            <img src="../../assets/add@2x.png" style="margin-right: 10px; display: inline;float:right;width:30px;" class="plus" @click="plus"/>
            <input pattern="[0-9]*" v-model="buy_count" type="text" name="counts" style="width:30px;display: inline;float:right;border: 0.5px solid #e2e2e2;line-height:28px;text-align:center;"/>
            <img src="../../assets/minus@2x.png" style="display: inline;float:right;width:30px;" class="minus" @click="minus"/>
          </div>
        </div>

        <section class="product_intro">
          <div class="pro_det" v-html="good.description" style='padding: 0 6.5px;'>
          </div>
        </section>
        </main>
      </div>

    <footer class="cart_d_footer">
      <div class="m">
        <ul class="m_box">
          <li class="m_item">
            <a @click="toCart" class="m_item_link">
              <em class="m_item_pic three"></em>
              <span class="m_item_name">購物車</span>
            </a>
          </li>
        </ul>
        <div class="btn_box clearfix" >
          <a @click="addToCart" class="buy_now">加入購物車</a>
          <a @click="zhifu" class="buybuy">立即購買</a>
        </div>
      </div>
    </footer>

    </div>
  </div>
</template>
<script>
import { go } from '../../libs/router'
//import { swiper, swiperSlide } from 'vue-awesome-swiper'
import {scrollPic} from '../../libs/index.js'

   export default{
        data(){
            return {
                good_images: [],
                good: "",
                buy_count: 1,
                good_id: this.$route.query.good_id,
            }
        },
        watch:{
        },
        mounted(){
          scrollPic();   //輪播圖

          this.$http.get(this.$configs.api + 'goods/goods_details?good_id=' + this.good_id).then((response)=>{
            console.info(this.good_id)
            console.info(response.body)
            this.good = response.body.good
            this.good_images = response.body.good_images

          },(error) => {
            console.error(error)
          });
        },
        methods:{
          addToCart () {
            alert("商品已經加入到了購物車")
            let goods = {
              id: this.good_id,
              title: this.good.name,
              quantity: this.buy_count,
              price: this.good.price,
              image: this.good_images[0]
            }
            this.$store.dispatch('addToCart', goods)
          },
          toCart () {
            go("/cart", this.$router)
          },
          plus () {
            this.buy_count = this.buy_count + 1
          },
          minus () {
            if(this.buy_count > 1) {
              this.buy_count = this.buy_count - 1
            }
          },
          zhifu () {
            go("/shops/dingdanzhifu?good_id=" + this.good_id + "&buy_count=" + this.buy_count, this.$router)
          },
        },
        components: {
        },
				computed: {
				}
    }
</script>
```

在上面的代碼中， 

1. 實現了加入購物車的方法
2. 實現了對於支付頁面的跳轉
3. 實現了從遠程接口讀取數據


## 3. 添加物品到購物車

下面的代碼是把某個商品添加到購物車中: 

```
addToCart () {
    console.info('加入購物車')
    alert("商品已經加入到了購物車")
    let goods = {
        id: this.good_id,
        title: this.good.name,
        quantity: this.buy_count,
        price: this.good.price,
        image: this.good_images[0]
    }
    this.$store.dispatch('addToCart', goods)
},
```

同時，在 src/vuex/actions.js中，添加如下代碼: 

```
export const addToCart = ({ commit }, product) => {
    commit(types.ADD_TO_CART, {
      id: parseInt(product.id),
      image: product.image,
      title: product.title,
      quantity: product.quantity,
      price: product.price
    })
}
```

## 看效果

![詳情頁](/images/real_project/show_page.png)

## 總結

- 購物車使用了Vuex來保存數據。 下一節會詳述。
- 進入到支付頁面，在後面會詳述。 這個頁面我們只加上一個鏈接就好了
- 本頁面使用了後臺提供的接口，會返回必要的數據。 接口結構略。 
