# 微信支付

微信支付，最難的地方不在於技術，而是在於微信有一套自己的技術規範。 

建議每位同學都要從官方文檔看起。 雖然市面上有一些集成工具，例如 Ping++ , 但是往往這些產品不是特別方便，收費也比較高昂。 出了問題不好調試。

所以，我們對於核心技術，一定要親自掌握。 

## 申請微信賬號和配置

這裏就不詳述了。 我們假設全部的賬號都已經做好了。 

## 1. 添加支付頁面的路由


```
import Pay from '@/components/pay'

Vue.use(Router)

export default new Router({
  routes: [
    { 
      path: '/shops/pay',
      name: 'Pay',
      component: Pay
    },
  ]

})
```

## 2. 添加vue頁面

```
<template>
  <div class="background">
    <header class="top_bar">
      <a onclick="window.history.go(-1)" class="icon_back"></a>
      <h3 class="cartname">訂單支付</h3>
    </header>

    <div class="tast_list_bd" style="background-color: #F3F3F3; padding-top: 0; padding-bottom: 80px;">
      <div class="goods_detail" style="">

        <main class="detail_box">
            <span class="divider"></span>

            <form style="margin-top: 45px;">
                <div class="column is-12">
                    <label class="label">收貨人</label>
                    <p class="control has-icon has-icon-right">
                    <input name="name" v-model="mobile_user_name" v-validate="'required|required'" :class="{'input': true, 'is-danger': errors.has('name') }" type="text" placeholder="例如: 張三" autofocus="autofocus"/>
                    <span v-show="errors.has('name')" class="help is-danger">收貨人不能爲空</span>
                    </p>
                </div>

                <div class="column is-12">
                    <label class="label">收貨地址</label>
                    <p class="control has-icon has-icon-right">
                    <input name="url" v-model="mobile_user_address" v-validate="'required|required'" :class="{'input': true, 'is-danger': errors.has('url') }" type="text" placeholder="例如: 北京市朝陽區大望路西西里小區4棟2單元201"/>
                    <span v-show="errors.has('url')" class="help is-danger">收貨地址不能爲空</span>
                    </p>
                </div>

                <div class="column is-12">
                    <label class="label">收貨電話</label>
                    <p class="control has-icon has-icon-right">
                    <input name="phone" v-model="mobile_user_phone" v-validate="'required|numeric'" :class="{'input': true, 'is-danger': errors.has('phone') }" type="text" placeholder="例如: 18888888888"/>
                    <span v-show="errors.has('phone')" class="help is-danger">電話號碼不能爲空</span>
                    </p>
                </div>
            </form>

        <span class="divider"></span>

        <section class="product_info clearfix" v-if="single_pay">
          <div>
            <div class="fu_li_zhuan_qu" >
              <img :src="good_images[0]" class="logo_image"/>
              <div class="content" >
                <div class="title">
                  {{good.name}}
                </div>
                <div class="logo_and_shop_name">
                  <div class="product_pric">
                    <span>￥</span>
                    <span class="rel_price">{{good.price}}</span>
                    <span> &nbsp x {{buy_count}}</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </section>

        <section class="product_info clearfix" v-else v-for="product in cartProducts">
          <div>
            <div class="fu_li_zhuan_qu" >
              <img :src="product.image" class="logo_image"/>
              <div class="content" >
                <div class="title">
                  {{product.title}}
                </div>
                <div class="logo_and_shop_name">
                  <div class="product_pric">
                    <span>￥</span>
                    <span class="rel_price">{{product.price}}</span>
                    <span> &nbsp x {{product.quantity}}</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </section>

        <section>
          <span class="divider" style="height: 15px;"></span>
          <div class="extra_cost" style=" ">
            <span style="float: left; margin-left: 15px;"> 賣家留言:</span>
            <input v-model="guest_remarks" id="extra_charge" type="text" name="cost" placeholder="選填: 對本次交易的說明" style="border: 0; background-color: white;
            font-size: 15px; color: #48484b; outline: none; width: 60%;"></input>
          </div>
        </section>

        <section>
          <span class="divider"></span>
          <div class="extra_cost" style=" ">
            <span style="float: left; margin-left: 15px;"> 應付金額:</span>
            <div v-if="single_pay" class="rel_price" type="text" name="cost" style="border: 0; background-color: white;
            font-size: 20px; color: #ff621a; font-weight: bold; outline: none; text-align: right; padding-right: 20px;"> \{\{ total_cost | currency }}</div>
            <div v-else class="rel_price" type="text" name="cost" style="border: 0; background-color: white;
            font-size: 20px; color: #ff621a; font-weight: bold; outline: none; text-align: right; padding-right: 20px;"> \{\{ total | currency }}</div>

          </div>
        </section>
        </main>

        <span class="divider"></span>

        <div style="height: 55px;  display: flex; width: 100%; padding: 0px 10px; background-color: #fff;" @click="">
          <div style="flex: 1; display: flex;">
            <div style="margin-top: 10px;">
              <img src="../../assets/微信icon@3x.png" style="width: 35px;"/>
            </div>
            <span style="margin-top: 8px; font-size: 18px; line-height:40px; margin-left: 10px;">微信支付</span>
          </div>

          <div style=" padding: 14px 10px;" @click="user_wechat">
            <img src="../../assets/選中3x.png" style="width: 28px;"/>
          </div>
        </div>
      </div>
    </div>

    <div class="shop_layout-scroll-absolute" style="">
      <div class="queding" @click="buy">
        立即支付
      </div>
    </div>

  </div>
</template>
<script>
   import { go } from '../../libs/router'
   import { mapGetters } from 'vuex'
   export default{
        data(){
            return {
                good_images: [],
                good: "",
                buy_count: this.$route.query.buy_count,
                good_id: this.$route.query.good_id,
                open_id: this.$store.state.userInfo.open_id,
                mobile_user_address: '',
                mobile_user_name: '',
                mobile_user_phone: '',
                guest_remarks: '',
                is_use_wechat: false,
            }
        },
        watch:{
        },
        mounted(){
          if (this.single_pay) {
              this.$http.get(this.$configs.api + 'goods/goods_details?good_id=' + this.good_id).then((response)=>{
                  console.info(this.good_id)
                  console.info(response.body)
                  this.good = response.body.good
                  this.good_images = response.body.good_images
              },(error) => {
                  console.error(error)
              });
          }
        },
        computed:  {
            total () {
              return this.cartProducts.reduce((total, p) => {
                return (total + p.price * p.quantity)
              }, 0)
            },
            single_pay () {
               return this.good_id && this.buy_count
            },
            total_cost () {
              return this.good.price * this.buy_count
            },
            ...mapGetters({
              cartProducts: 'cartProducts',
              checkoutStatus: 'checkoutStatus'
            })
        },
        methods:{
            validateBeforeSubmit() {
               //攔截異步操作
               return new Promise((resolve, reject) => {
                   this.$validator.validateAll().then(result => {
                       console.info(result)
                       if (result) {
                           console.info("============表單驗證成功===")
                           resolve(true);
                       } else {
                           alert('請填寫完整的收貨信息!');
                           resolve(false);
                       }
                    });
               })
            },
            plus () {
              this.buy_count = this.buy_count + 1
            },
            minus () {
              if(this.buy_count > 1) {
                this.buy_count = this.buy_count - 1
              }
            },
            user_wechat () {
              if (this.is_use_wechat === false) {
                this.is_use_wechat = true
              } else {
                this.is_use_wechat = false
              }
            },
            buy (){
                let result = this.validateBeforeSubmit().then((resolve)=>{
                    if (resolve) {
                        console.info('true ==== ')
                        let params
                        if (this.single_pay) {
                            params = {
                                good_id: this.good_id,
                                buy_count: this.buy_count,
                                total_cost: this.total_cost,
                                guest_remarks: this.guest_remarks,
                                mobile_user_address: this.mobile_user_address,
                                mobile_user_name: this.mobile_user_name,
                                mobile_user_phone: this.mobile_user_phone,
                                open_id: this.open_id
                            }
                        } else {
                            console.info(this.total)
                            params = {
                                goods: this.cartProducts,
                                total_cost: this.total,
                                guest_remarks: this.guest_remarks,
                                mobile_user_address: this.mobile_user_address,
                                mobile_user_name: this.mobile_user_name,
                                mobile_user_phone: this.mobile_user_phone,
                                open_id: this.open_id
                            }
                        }
                        this.$http.post(this.$configs.api + 'goods/buy', params
                        ).then((response) => {
                            let order_number =  response.body.order_number
                            this.purchase(order_number)
                        }, (error) => {
                            console.error(error)
                        });
                    } else {
                        console.info('== 請填寫完整的收貨信息')
                    }
                });
            },
            purchase (order_number) {
              //調起微信支付界面
              if (typeof WeixinJSBridge == "undefined"){
                if( document.addEventListener ){
                  document.addEventListener('WeixinJSBridgeReady', this.onBridgeReady, false);
                }else if (document.attachEvent){
                  document.attachEvent('WeixinJSBridgeReady', this.onBridgeReady);
                  document.attachEvent('onWeixinJSBridgeReady', this.onBridgeReady);
                }
              }else{
                this.onBridgeReady(order_number);
              }
            },
            onBridgeReady (order_number) {
              let that = this
              let total_cost
              if (this.single_pay) {
                total_cost = this.total_cost
              } else {
                total_cost = this.total
              }
              this.$http.post(this.$configs.api + 'payments/user_pay',
              {
                open_id: this.$store.state.userInfo.open_id,
                total_cost: total_cost,
                order_number: order_number
              }).then((response) => {
                WeixinJSBridge.invoke(
                    'getBrandWCPayRequest', {
                        "appId": response.data.appId,
                        "timeStamp": response.data.timeStamp,
                        "nonceStr": response.data.nonceStr,
                        "package": response.data.package,
                        "signType": response.data.signType,
                        "paySign":  response.data.paySign
                    },
                    function(res){
                        // 下面代碼僅用於調試
                        // alert("res.err_msg: " + res.err_msg + ", err_desc: " + res.err_desc)
                        if(res.err_msg == "get_brand_wcpay_request:ok" ) {
                          // 使用以上方式判斷前端返回,微信團隊鄭重提示：res.err_msg將在用戶支付成功後返回    ok，但並不保證它絕對可靠。
                          that.$router.push({ path: '/shops/paysuccess?order_id=' + order_number });
                        } else {
                          // 顯示取消支付或者失敗
                          that.$router.push({ path: '/shops/payfail?order_id=' + order_number });
                        }
                    }
                );
              }, (error) => {
                console.error(error)
              });
            }
          },
    }
</script>
```

核心代碼如下：

```
onBridgeReady (order_number) {
  //....
  this.$http.post(this.$configs.api + 'payments/user_pay',
  {
    open_id: this.$store.state.userInfo.open_id,
    total_cost: total_cost,
    order_number: order_number
  }).then((response) => {
    WeixinJSBridge.invoke(
        'getBrandWCPayRequest', {
            "appId": response.data.appId,
            "timeStamp": response.data.timeStamp,
            //....
        },
        function(res){
            //...
        }
    );
  }, (error) => {
    console.error(error)
  });
}
```

上面的代碼是用來給頁面一準備好( WeixinJSBridge 準備好了)的時候，頁面就要調用的。 


```
purchase (order_number) {
  //調起微信支付界面
  if (typeof WeixinJSBridge == "undefined"){
    if( document.addEventListener ){
      document.addEventListener('WeixinJSBridgeReady', this.onBridgeReady, false);
    }else if (document.attachEvent){
      document.attachEvent('WeixinJSBridgeReady', this.onBridgeReady);
      document.attachEvent('onWeixinJSBridgeReady', this.onBridgeReady);
    }
  }else{
    this.onBridgeReady(order_number);
  }
},
```            

上面的代碼用於調用出 “微信支付頁面”.  其中的變量 `WeixinJSBridge` 是微信瀏覽器自帶的變量， 不必聲名，直接拿過來用就行。

## 看效果

微信的支付頁面會跳出來 （圖略）

## 總結

可以看到：

1. 微信支付的細節處理，都交給了後臺服務器端。 只要我們H5端把參數準備好，直接訪問  http://shopweb.siwei.me/api/payments/user_pay 就可以了。 
2. 微信支付，分成單筆商品支付和多筆商品支付兩種情況. 區別就是把參數重新組織一下即可。
3. 新手對於 WeixinJSBridge 這個變量很難掌握， 一定要多看文檔。 這個文檔是 “微信公衆號內支付”的文檔， 不是微信APP ， 或者微信普通H5的文檔。 一定要梳理好邏輯。 另外，對於後端API的同學這裏更難，建議多查多試。 
4. 在微信的後臺，要配置不同的支付目錄。 安卓和IOS 的粗略是不一樣的。 建議大家百度一下。
5. 微信的支付場景對應的支付方式和實現方式是不一樣的。 本例是“微信的公衆號內支付”。

微信的官方文檔中， 提供的例子都是基於經典的WEB頁面（整體刷新的那種）的，目前還沒有看到SPA的例子。 但是大家的問題很多。 我的個人博客也記錄了一些內容： http://siwei.me/blog/posts/--27

由於篇幅限制，微信相關的內容不再贅述。