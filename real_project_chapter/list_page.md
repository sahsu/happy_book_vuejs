# 商品列表頁

商品列表， 在我們的首頁有一部分， 在“列表頁”（第二個Tab)也會存在。 

所以我們可以直接把抽取成爲組件。 

下面以首頁中引用爲例：

## 1. 在首頁中添加代碼 

```
<template>
  <div class="background">
    <div class="home">
      <div class="m_layout">
        <div class="product_top">
          <div class="product_left">
            <div>商品列表</div>
          </div>
        </div>
        <span class="divider" style="height: 2px;"></span>
        <!-- 這裏循環顯示特產商品列表 -->
        <SpecialMarket :id="good.id" :name="good.name" :description="good.description" :image_url="good.image_url" v-for="good in goods"></SpecialMarket>
      </div>
    </div>
    <NavBottomView :is_shops_index="is_shops_index"></NavBottomView>
  </div>
</template>
<script>
    // 在這裏引入 特產component
    import SpecialMarket from '../../components/SpecialMarket.vue';
</script>    
```

上面的核心代碼按如下：

```
<!-- 這裏循環顯示特產商品列表 -->
<SpecialMarket :id="good.id" :name="good.name" :description="good.description" :image_url="good.image_url" v-for="good in goods"></SpecialMarket>
```

使用了v-for 和 componment的組合，來顯示列表。

## 2. 在component中添加該文件

新增文件 src/components/SpecialMarket.vue: 

```
<template>
  <div>
    <div @click="show_goods_details" class="fu_li_zhuan_qu" >
      <img :src="image_url" class="logo_image"/>
      <div class="content" >
        <div class="title">
          {{name}}
        </div>
        <div class="logo_and_shop_name">
          <span v-html="description"></span>
        </div>
      </div>
    </div>
    <span class="divider" style="height: 2px;"></span>
  </div>
</template>
<script>
import { go } from '../libs/router'

    export default{
        data(){
            return {
            }
        },
        props: {
          id: Number,
          name: String,
          description: String,
          image_url: String,
        },
        mounted(){
        },
        methods:{
         show_goods_details () {
           go("/shops/goods_details?good_id=" + this.id, this.$router)
         },
        },
        components:{
        },
    }
</script>

```

可以看到，該段代碼會接受一個數組，然後循環顯示。 點擊任意一個按鈕， 跳轉到詳情頁面。

## 總結

這裏算是最簡單的地方了。 