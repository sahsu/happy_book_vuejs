# 購物車

購物車具備兩個功能： 

1. 保存用戶需要的數據
2. 清空

所以，我們使用vuex來實現購物車

## 1. 添加購物車 路由

向 文件 src/router/index.js 中增加內容：  

```
import Cart from '@/components/Cart'

Vue.use(Router)

export default new Router({
  routes: [
    { path: '/cart',
      name: 'Cart',
      component: Cart
    },
  ]
})
```

## 2. 添加查看購物車的vue頁面

新增 src/components/Car.vue 文件，內容如下： 

```
<template>
  <div class="background">
    <div id="my_cart">
      <CartHeaderView></CartHeaderView>
      <CartMainView></CartMainView>
      <NavBottomView :is_cart="is_cart"></NavBottomView>
    </div>
  </div>
</template>

<script>

 import CartHeaderView from './CartHeader.vue';
 import CartMainView from './CartMain.vue';
 import NavBottomView from './NavBottom.vue';

 export default{
   data () {
     return {
       is_cart: true
     }
   },
   mounted(){
   },
   components:{
     CartHeaderView,
     CartMainView,
     NavBottomView
   }
 }
</script>

```

## 3. 增加對應的組件

3.1 新增購物車的頭部文件  src/components/CartHeader.vue: 

```
<template>
	<div id="carttp">
		<header class="top_bar">
	    <a onclick="window.history.go(-1)" class="icon_back"></a>
	    <h3 class="cartname">購物車</h3>
	</header>
	</div>
</template>
```

3.2 新增購物車的主體內容 src/components/CartMain.vue: 

```
<template>
    <main class="cart_box">
    <p v-show="!products.length"><i>請選擇商品加入到購物車</i></p>
        <div class="cart_content clearfix" v-for="item in products" style="position: relative;">
            <div class="cart_shop clearfix">
                <div class="cart_check_box">
                    <div class="check_box" checked>
                    </div>
                </div>
                <div class="shop_info clearfix">
                    <span class="shop_name" style="font-size: 14px;">絲路樂購新疆商城</span>
                </div>
            </div>

            <div @click="find_item_id(item)" class="cart_del clearfix" style="display: inline-block; position: absolute; top: 10px; right: 10px;">
              <div class="del_top">
              </div>
              <div class="del_bottom">
              </div>
            </div>
            <div class="cart_item">
                <div class="cart_item_box">
                    <div class="check_box">
                    </div>
                </div>
                <div class="cart_detial_box clearfix">
                    <a class="cart_product_link">
                        <img :src="item.image" alt="">
                    </a>
                    <div class="item_names">
                        <a>
                            <span>{{item.title}}</span>
                        </a>
                    </div>
                    <div class="cart_weight">
                        <span class="my_color" style="color: #979292;">{{item.title}}</span>
                    </div>
                    <div class="cart_product_sell">
                        <span class="product_money">￥<strong class="real_money">{{item.price}}</strong></span>
                        <div class="cart_add clearfix">
                            <span @click="minus(item.id)" class="my_jian">-</span>
                            <input type="tel" class="my_count" :value="item.quantity">
                            <span @click="add(item.id)" class="my_add">+</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="pop" style="display: none">
          <div class="pop_box">
            <div class="del_info">
              確定要刪除該商品嗎？
            </div>
            <div class="del_cancel">
              取消
            </div>
            <div @click="deleteItem" class="del_ok">
              確定
            </div>
          </div>
        </div>

        <div class="cart_fo">
          <footer class="cart_footer">
            <div class="count_money_box">
              <div class="heji">
                <strong>合計:</strong>
                <strong style="color: #ff621a; font-size: 18px;">\{\{ total | currency }}</strong>
              </div>
              <a :disabled="!products.length" @click="checkout(products)" class="go_pay">
                <span style="color: #f5f5f5; font-weight: 600;">結算</span>
              </a>
            </div>
          </footer>
        </div>
    </main>
</template>
<script>
 import { mapGetters } from 'vuex'
 import { go } from '../libs/router'
 import {check,animatDelBox} from '../assets/js/cart.js'

    export default{
      data(){
        return{
          need_delete_item: {},
          cartDatas:[ ],
        }
      },
    mounted(){
      check();
      animatDelBox();
    },
    computed: {
      ...mapGetters({
        products: 'cartProducts',
        checkoutStatus: 'checkoutStatus'
      }),
      total () {
        return this.products.reduce((total, item) => {
          return total + item.price * item.quantity
        }, 0)
      }
    },
    methods: {

      // 跳轉到支付頁面
      checkout (products) {
        go("/shops/dingdanzhifu", this.$router)
      },

      // 對於商品的數量進行增加
      add (id) {
        this.$store.dispatch('changeItemNumber', {id, type: 'add'})
      },

      // 對於商品的數量進行減少
      minus (id) {
        this.$store.dispatch('changeItemNumber', {id, type: 'minus'})
      },

      // 刪除某個商品
      deleteItem () {
        this.$store.dispatch('deleteItem', this.need_delete_item.id)
      },
      find_item_id (item) {
        this.need_delete_item = item
      }
    },
    }
</script>
```

3.3 修改Vuex的函數

把下面代碼添加到 src/vuex/actions.js 文件中: 

```
export const deleteItem = ({ commit }, id) => {
  commit(types.DELETE_ITEM, {
    id: parseInt(id)
  })
}

export const changeItemNumber = ({ commit }, {id, type}) => {
  console.info(id)
  commit(types.CHANGE_ONE_QUANTITY, {
    id: parseInt(id),
    type
  })
}

```

上面的代碼， 實現了購物車的若干功能： 

- 對於商品數量的增減
- 實現了當商品數量改變時，商品總價也跟着修改。 

## 看效果

購物車做好後，點擊打開，如下圖所示：

![購物車頁面](/images/real_project/cart.png)

## 總結

- 購物車的數據， 是通過Vuex來保存的
- 購物車中數量的加減，是需要直接影響到商品總價的。 這個角度來看，用Vuex來做更加適合。
