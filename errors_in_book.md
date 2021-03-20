# 實體書的勘誤表

非常抱歉，由於我的個人失誤，草稿以markdown格式書寫，在複製粘貼的時候，代碼中的 `{{ }}` 沒有複製到 word中。

不過在code_example文件夾中的中都很完整。讀者可以直接運行那邊的代碼。

## 2.1 極速入門(實體書第10頁)

`{{show_my_text}}`在例子中缺失

錯誤的內容:

```
<div id='app'>

</div>
```

正確的內容應該是:
```
<div id='app'>
  {{show_my_text}}
</div>
```

## 2.2.2 HTML代碼的<head>部分(實體第19頁)

章節號標記錯誤

錯誤的原文:

```
// 下面的window.onload函數暫時省略。 會在 2.2.3 js代碼部分講解到。
...
  <!-- 這裏的代碼暫時省略。會在 2.2.2 HTML代碼的body部分中講解到 -->
```

正確的內容應該是:

```
// 下面的window.onload函數暫時省略。 會在 2.2.4 js代碼部分講解到。
...
  <!-- 這裏的代碼暫時省略。會在 2.2.3 HTML代碼的body部分中講解到 -->

```


## 4.1.4 定義並顯示變量 (實體書第54頁 )

代碼缺失

錯誤的原文:

```
<template>
  <div>
  <!--  步驟2: 在這裏顯示 message -->

  </div>
</template>
```

正確的內容應該是:

```
<template>
  <div>
  <!--  步驟2: 在這裏顯示 message -->
  {{message}}
  </div>
</template>
```


## 4.4.1 渲染某個變量(實體書64)

代碼缺失

錯誤的原文:

```
可以這樣來顯示它:
<div></div>

完整代碼如下:
<template>
  <div>

  </div>
</template>

```


正確的內容應該是:

```
可以這樣來顯示它:
<div>{{my_value}}</div>

完整代碼如下:
<template>
  <div>
    {{message}}
  </div>
</template>
```

## 4.5.6 v-on (實體書73頁)

代碼缺失.

錯誤的原文:

```
<body>
  <div id='app'>

    <br/>
```

正確的內容應該是:


```
<body>
  <div id='app'>
    {{message}}
    <br/>
```

## 4.7.2 顯示博客詳情頁 (實體書87頁)

代碼缺失

錯誤的原文:

```
<div>
  <p> 標題: </p>
  <p> 發佈於: </p>
  <div>

  </div>
</div>
```

正確的內容應該是:

```
<div>
  <p> 標題: {{blog.title}} </p>
  <p> 發佈於: {{blog.created_at}} </p>
  <div>
    {{blog.body}}
  </div>
</div>

```

## 4.7.5 修改博客列表頁的跳轉方式2: 使用v-link (實體書91頁)

代碼缺失

錯誤的原文:

```
<td>
  <router-link :to="{name: 'Blog', query: {id: blog.id}}">

  </router-link>
</td>
```

正確的內容應該是:

```
<td>
  <router-link :to="{name: 'Blog', query: {id: blog.id}}">
    {{blog.id}}
  </router-link>
</td>
```

## 4.10 雙向綁定(實體書96頁)


代碼缺失

錯誤的原文:

```
    <!-- 顯示　this.my_value 這個變量 -->
    <p>頁面上的值:   </p>
```


正確的內容應該是:


```
    <!-- 顯示　this.my_value 這個變量 -->
    <p>頁面上的值: {{my_value}}  </p>
```


## 4.11 表單項目的綁定(實體書99-100頁)



代碼缺失

錯誤的原文:
```
<template>
  <div>
    input： <input type='text' v-model="input_value"/>,
    輸入的值：
    <hr/>
    text area： <textarea v-model="textarea_value"></textarea>,
    輸入的值：
    <hr/>
    radio:
    <input type='radio' v-model='radio_value' value='A'/> A,
    <input type='radio' v-model='radio_value' value='B'/> B,
    <input type='radio' v-model='radio_value' value='C'/> C,
    輸入的值：

    <hr/>
    checkbox:
    <input type='checkbox' v-model='checkbox_value'
      v-bind:true-value='true'
      v-bind:false-value='false'
      /> ,
    輸入的值：

    <hr/>
    select:
    <select v-model='select_value'>
      <option v-for="e in options" v-bind:value="e.value">
        {{e.text}}
      </option>
    </select>
    輸入的值：
  </div>
</template>
```


正確的內容應該是:

```
<template>
  <div>
    input： <input type='text' v-model="input_value"/>,
    輸入的值：{{input_value}}
    <hr/>
    text area： <textarea v-model="textarea_value"></textarea>,
    輸入的值：{{textarea_value}}
    <hr/>
    radio:
    <input type='radio' v-model='radio_value' value='A'/> A,
    <input type='radio' v-model='radio_value' value='B'/> B,
    <input type='radio' v-model='radio_value' value='C'/> C,
    輸入的值：
    {{radio_value}}
    <hr/>
    checkbox:
    <input type='checkbox' v-model='checkbox_value'
      v-bind:true-value='true'
      v-bind:false-value='false'
      /> ,
    輸入的值：
    {{checkbox_value}}
    <hr/>
    select:
    <select v-model='select_value'>
      <option v-for="e in options" v-bind:value="e.value">
        {{e.text}}
      </option>
    </select>
    輸入的值：{{select_value}}
  </div>
</template>
```

## 6.3.1 典型例子 (實體書135頁)



代碼缺失

錯誤的原文:

```
<div id='app'>
    <p> 原始字符串： </p>
    <p> 通過運算後得到的字符串： </p>
</div>
```


正確的內容應該是:

```
<div id='app'>
    <p> 原始字符串： {{my_text}} </p>
    <p> 通過運算後得到的字符串：{{my_computed_text}} </p>
</div>

```

## 6.3.2 Computed Properties 與普通方法的區別(實體書137頁)



代碼缺失

錯誤的原文:

```
    <div id='app'>
        <p> 原始字符串：  </p>
        <p> 通過運算後得到的字符串： {{my_computed_text() }} </p>
    </div>
```


正確的內容應該是:

```
    <div id='app'>
        <p> 原始字符串：{{my_text}} </p>
        <p> 通過運算後得到的字符串： {{my_computed_text() }} </p>
    </div>
```

## 6.3.3 watched properties (實體書138,139頁)



代碼缺失

錯誤的原文:

```
(138頁)
<p> 我所在的詳細一些的地址：  （每次其他兩個發生變化，這裏就會跟着變化) </p>
(139頁)
<p> 我所在的詳細一些的地址：  （這是使用computed 實現的版本) </p>
```


正確的內容應該是:

```
(138頁)
<p> 我所在的詳細一些的地址：{{full_address}}  （每次其他兩個發生變化，這裏就會跟着變化) </p>
(139頁)
<p> 我所在的詳細一些的地址：{{full_address}}  （這是使用computed 實現的版本) </p>

```

## 8.5 用戶的註冊和微信授權(實體書196頁)


錯誤的原文:

```
if(this。user_info。open_id)
```


正確的內容應該是:

```
if(this.user_info.open_id)
```
## 8.9 商品列表頁 (實體書218頁)


代碼缺失

錯誤的原文:

```
<div class="title">

</div>
```


正確的內容應該是:

```
<div class="title">
  {{name}}
</div>
```
## 8.10 商品詳情頁 (實體書220頁)


代碼缺失

錯誤的原文:

```
    <p class="p_name"></p>
    <div class="product_pric">
      <span>￥</span>
      <span class="rel_price"></span>
      <span></span>

      <span style='color: grey;
      text-decoration: line-through;
      font-size: 18px;
      margin-left: 14px;'>
        原價: ￥
      </span>
    </div>
```


正確的內容應該是:

```
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

```


## 8.11 購物車(實體書228頁)


代碼缺失

錯誤的原文:

```
<div class="item_names">
    <a>
        <span></span>
    </a>
</div>
<div class="cart_weight">
    <span class="my_color" style="color: #979292;"></span>
</div>
<div class="cart_product_sell">
    <span class="product_money">￥<strong class="real_money"></strong></span>
```


正確的內容應該是:

```
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

```

## 8.13 微信支付（實體書235頁)

代碼缺失

錯誤的原文:

```
<div class="title">

</div>
<div class="logo_and_shop_name">
  <div class="product_pric">
    <span>￥</span>
    <span class="rel_price"> </span>
    <span> &nbsp x  </span>
  </div>
</div>

```


正確的內容應該是:

```
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

```

## 8.13 微信支付（實體書236頁)

代碼缺失

錯誤的原文:

```
<div class="title">

</div>
<div class="logo_and_shop_name">
  <div class="product_pric">
    <span>￥</span>
    <span class="rel_price"> </span>
    <span> &nbsp x  </span>
  </div>
</div>
```


正確的內容應該是:

```
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
```
