# 不同頁面間的參數的傳遞

在普通的web開發中，參數傳遞有以下幾種形式：

1. url:   `/another_page?id=3`
2. 表單: `<form>...</form>`

而在Vuejs中，不會產生表單的提交（這會引起頁面的整體刷新）.　所以有兩種：

1. url ．　同傳統語言． 參數體現在url中。
2. vuejs 內部的機制．（無法在url 中體現，可以認爲是由js代碼隱式實現的）

我們用一個實際的例子說明．

我們之前實現了　＂博客列表頁＂，接下來我們要實現：點擊博客列表頁中的某一行，就顯示博客詳情頁．

## 1. 回顧：現有的接口

我已經做好了一個接口： 文章詳情頁。 地址爲：  

`/interface/blogs/show`

該接口接收一個參數： id.   使用 http GET請求來訪問。

下面是該接口的一個完整形式：

http://siwei.me/interface/blogs/show?id=1244

下面是返回結果: 

```
{
    "result":{
        "body":"<p>這個問題很常見，解決辦法就是禁止硬件加速...</p>",
        "id":1244,
        "title":"android - 在 view pager中的 webview, 切換時，會閃爍的問題。"這個問題很常見，解決辦法就是禁止硬件加速...</p>
    }
}
```

在瀏覽器中看起來如下圖所示： 

![blog show interface](./images/siwei_blog_inteface_show.png)

## 2. 顯示博客詳情頁

我們需要新增Vue頁面：  `src/components/Blog.vue`, 用於顯示博客詳情．

```
<template>
  <div >
    <div>
      <p> 標題： {{ blog.title }}  </p>
      <p> 發佈於： {{blog.created_at }}</p>
      <div>
       {{ blog.body }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      blog: {}
    }
  },
  mounted() {
    this.$http.get('api/interface/blogs/show?id='+this.$route.query.id).then((response) => {
       this.blog = response.body.result
    }, (response) => {
       console.error(response)
    });
  }
}
</script>

<style>
</style>
```

上面代碼中:

- `data(){ blog: {}}` 用來初始化 blog這個頁面用到的變量.

- `{{blog.body}}`, `{{blog.title}}` 等, 用來顯示blog相關的信息.

- `mounted...` 中,定義了發起http的請求.

- `this.$route.query.id` 獲取url 中的id參數. 例如:   `/my_url?id=333` , 那麼 '333' 就是取到的結果. 具體的定義看下面的路由。

## 3. 新增路由

修改 ： `src/router/index.js`文件. 添加如下代碼： 

```
import Blog from '@/components/Blog'

export default new Router({
  routes: [
		// ...
    {
      path: '/blog',
      name: 'Blog',
      component: Blog
    }
  ]
} )
```

上面的代碼，就是讓Vuejs 可以對形如  `http://localhost:8080/#/blog` 的url進行處理。 對應的vue文件是 `@/components/Blog.vue`.

## 4. 修改博客列表頁--跳轉方式1: 使用事件

我們需要修改博客列表頁, 增加跳轉事件:

修改：`src/components/BlogList.vue`, 如下代碼所示：

```
<template>
  ...
      <tr v-for="blog in blogs">
        <td @click='show_blog(blog.id)'>{{blog.title }}</td>
      </tr>
  ...
</template>
<script>
export default {
  methods: {
    show_blog: function(blog_id) {
      this.$router.push({name: 'Blog', query: {id: blog_id}})
    }
  }
}
</script>
```

在上面代碼中,

- `<td @click='show_blog(blog.id)'...</td>`  表示:  該`<td>`標籤在被點擊的時候,會觸發一個事件: `show_blog`, 
並且以當前正在遍歷的 blog對象的id 作爲參數.

- `methods: {}` 是比較核心的方法,  vue頁面中用到的事件,都要寫在這裏.

- `show_blog: function...` 就是我們定義的方法.該方法可以通過`@click="show_blog" `來調用.

- `this.$router.push({name: 'Blog', params: {id: blog_id}})`中:

  - `this.$router` 是vue的內置對象. 表示路由.
  - `this.$router.push` 表示讓vue跳轉. 跳轉到 name: Blog 對應的vue頁面. 參數是 id: blog_id .


## 演示

(TODO 這裏的GIF要換成靜態圖片)
打開“博客列表頁” ，就可以看到對應的所有文章。然後點擊其中一個的標題，就可以打開對應的文章詳情頁了。 

如下圖GIF動畫所示： 

![點擊之後的跳轉動畫](./images/點擊後的跳轉.gif)

## 不經過HTML轉義,直接打印結果.

我們發現，ＨＴＭＬ的源代碼在頁面顯示的時候被轉義了，如下:

![轉義後的html亂碼](./images/vue打開博客詳情頁-沒有做美化和格式化.png)

所以，我們把它修改一下,不要轉義:

```
<template>
   .....
      <div v-html='blog.body'>
      </div>
   .....
</template>
```

上面的 `v-html` 就表示不轉義.

效果如下圖:

![不轉義的效果](./images/vue_with_v_html.png)

## 修改博客列表頁--跳轉方式2: 使用v-link

`<router-link>` 比起 `<a href="...">` 會好一些，理由如下：

無論是 HTML5 history 模式還是 hash 模式，它的表現行爲一致，所以，當你要切換路由模式，或者在 IE9 降級使用 hash 模式，無須作任何變動。

在 HTML5 history 模式下，router-link 會攔截點擊事件，讓瀏覽器不再重新加載頁面。

當你在 HTML5 history 模式下使用 base 選項之後，所有的 to 屬性都不需要寫（基路徑）了。

```
<td>
  <router-link :to="{name: 'Blog', query: {id: blog.id}}">
    {{blog.id}}
  </router-link>
</td>
```

然後,就可以看到,生成的HTML形如:

```
<a href="#/blog?id=1239" class="">
	1239
</a>
```

點擊之後,有同樣的跳轉功能.

感興趣的同學，可以查看:  https://router.vuejs.org/zh-cn/api/router-link.html