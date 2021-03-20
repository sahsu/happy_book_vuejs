# 發送http請求

TODO:  需要加上 http resource, 在 main.js。

只要有js的地方，就要有接口。 特別是我們這樣前後端分離的SPA， 幾乎每個頁面都要發起http請求。從後臺接口讀取數據，並且顯示在前臺頁面。

這就需要用到http請求了. 

## 1. 調用http請求

vuejs 內置了對發送http請求的支持. 只需要在對應頁面的script 標籤內加上對應的代碼就好.
例如:

我們新增一個頁面,叫 "博客列表頁" :  `src/components/BlogList.vue`, 它的作用是從我的個人網站(http://siwei.me) 上，讀取文章的標題，並且顯示出來。

代碼如下:

```
<template>
  <div >
    <table>
      <tr v-for="blog in blogs">
        <td>{{blog.title }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data () {
    return {
      title: '博客列表頁',
      blogs: [
      ]
    }
  },
  mounted() {
    this.$http.get('api/interface/blogs/all').then((response) => {
       console.info(response.body)
       this.blogs = response.body.blogs
    }, (response) => {
       console.error(response)
    });
  }
}
</script>

<style >

td {
  border-bottom: 1px solid grey;
}
</style>
```

上面的代碼中， 我們先看 `<script/>`代碼段，

```
export default {
  data () {
    return {
      title: '博客列表頁',
      blogs: [
      ]
    }
  },
  mounted() {
    this.$http.get('api/interface/blogs/all').then((response) => {
       console.info(response.body)
       this.blogs = response.body.blogs
    }, (response) => {
       console.error(response)
    });
  }
}
```

上面代碼中，先是定義了兩個變量：  `title`,  `blogs`, 然後定義了一個 `mounted` 方法。該方法表示當頁面加載完畢後應該做哪些事情。
是一個鉤子方法。 

```
this.$http.get('api/interface/blogs/all').then((response) => {
   console.info(response.body)
   this.blogs = response.body.blogs
}, (response) => {
   console.error(response)
});
```    

上面代碼，是發起http請求的核心代碼。 訪問的接口地址是 `api/interface/blogs/all` ， 然後使用 `then`方法做下一步的事情， 
`then`方法接受兩個函數作爲參數，第一個是成功後幹嘛，第二個是失敗後幹嘛。 

成功後的代碼如下：  

```
this.blogs = response.body.blogs
```

然後，在對應的視圖部分顯示： 

```
<tr v-for="blog in blogs">
  <td>{{blog.title }}</td>
</tr>
```

## 2. 遠程接口的格式

在我的服務器上，讀取個人博客標題的接口我已經提前做好了，是 ：

http://siwei.me/interface/blogs/all

內容如下;

```
{
  blogs: [
    {
      id: 1516,
      title: "網絡安全資源",
      created_at: "2018-06-24T09:36:20+08:00"
    },
    {
      id: 1515,
      title: "github - 邀請夥伴後，需要修改權限",
      created_at: "2018-06-20T15:03:33+08:00"
    },
    {
      id: 1514,
      title: "ruby/rails - 根據瀏覽器的語言，來自動識別",
      created_at: "2018-06-19T08:28:44+08:00"
    }, 
    {
      id: 1513,
      title: "google cloud - 申請VM的經驗",
      created_at: "2018-06-09T16:42:08+08:00"
    },
    {
      id: 1512,
      title: "驗證碼 - 使用geetest 或者網易雲盾提供的動態二維碼",
      created_at: "2018-06-07T09:28:14+08:00"
    }
    // 更多內容。。。
  ]
}
```

在瀏覽器中打開後，如下圖所示（使用了 jsonview 插件做了json 的代碼格式化）: 

![siwei blog list api](./images/siwei_blog_interface_list.png)

## 3. 設置 Vuejs 開發服務器的代理

正常來說， javascript在瀏覽器中是無法發送跨域請求的，所以我們需要在vuejs的＂開發服務器＂上做個轉發配置．

修改：  `config/index.js`文件，增加下列內容：

```
module.exports = {
  dev: {
    proxyTable: {
      '/api': {        // 1. 對於所有以  "/api"　開頭的url 做處理．
        target: 'http://siwei.me',   // 3. 轉發到 siwei.me 上．
        changeOrigin: true,
        pathRewrite: {
          '^/api': ''  // 2. 把url中的　"/api" 去掉．
        }
      }
    },
}
```

上面的代碼做了三件事：

1. 對於所有以  "/api"　開頭的url 做處理．
2. 把url中的　"/api" 去掉．
3. 把新的url 請求打到　siwei.me 上．

例如：　
- 原請求：　　http://localhost:8080/api/interface/blogs/all
- 新請求：　　http://siwei.me/interface/blogs/all

注意：　以上的代理服務器內容，只能在＂開發模式＂下才能使用．在生產模式下，只能靠服務器的nginx的特性來解決js跨域問題．

修改後的 `config/index.js`文件的完整內容如下：

```
var path = require('path')

module.exports = {
  build: {
    env: require('./prod.env'),
    index: path.resolve(__dirname, '../dist/index.html'),
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    productionSourceMap: true,
    productionGzip: false,
    productionGzipExtensions: ['js', 'css'],
    bundleAnalyzerReport: process.env.npm_config_report
  },
  dev: {
    env: require('./dev.env'),
    port: 8080,
    autoOpenBrowser: true,
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',

    proxyTable: {
      '/api': {
        target: 'http://siwei.me',
        changeOrigin: true,
        pathRewrite: {
          '^/api': ''
        }
      }
    },

    cssSourceMap: false
  }
}

```

重啓服務器，可以看到我們的轉發設置已經生效：

```
$ npm run dev
...
[HPM] Proxy created: /api  ->  http://siwei.me
[HPM] Proxy rewrite rule created: "^/api" ~> ""
> Starting dev server...
...

```

## 4. 打開頁面，查看http請求

我們接下來，訪問  `http://localhost:8080/#/blogs/`

打開chrome developer tools, 就可以看到，"Network"中，已經有請求發出去了，截圖顯示了結果：

![chrome dev tools中的network](./images/vue_chrome_dev_tool_network.png)

另外，我們也可以直接在瀏覽器中，輸入要打開的鏈接，看到結果．(該瀏覽器使用了 json view插件)

![瀏覽器直接打開鏈接](./images/直接用瀏覽器打開要被代理服務器發出的請求.png)

## 5. 把結果渲染到頁面中．

我們發現，在export代碼段中，有兩個部分：

```
<script>
export default {
  data () { },
  mounted() { }
}
</script>

```
實際上，上面代碼中，　

- `data`方法，是用於＂聲明頁面會出現的變量＂，並且賦予初識值.(非常重要，切記這一點)
- `mounted` 表示頁面被vue渲染好之後的鉤子方法，會立刻執行．

所以，我們要把發送http的請求，寫到mounted方法中．（鉤子方法還有created, 我們可以暫且認爲
mounted方法與created方法基本一樣，一般我們在Vue 2.0中都使用mounted. 後續會說到區別. ）


```
  mounted() {
    this.$http.get('api/interface/blogs/all').then((response) => {
       this.blogs = response.body.blogs
    }, (response) => {
       console.error(response)
    });
  }
```

上面代碼中：

- `this.$http` 中的
- `this` 表示當前的vue組件（也即 BookList.vue)
- `$http` 所有以 `$`開頭的變量，都是vue的特殊變量，往往是vue框架自帶． 這裏的$http就是可以發起http
請求的對象.
- `$http.get`  是一個方法，可以發起get 請求．　只有一個參數就是目標url,
- `then()` 方法，來自於promise, 可以把異步的請求寫成普通的非異步形式．
第1個參數是成功後的callback,
第2個參數是失敗後的callback．
- `this.blogs = response.body.blogs` 中，是把遠程返回的結果（json ），賦予到本地．　由於javascript
的語言特性，能直接支持json ，所以纔可以這樣寫．

然後，我們通過這個代碼進行渲染：

```
<tr v-for="blog in blogs">
  <td>{{blog.title }}</td>
</tr>
```

在上面的代碼中：

- `v-for`是一個循環語法，可以把<tr>這個元素進行循環． 注意：這個叫directive,　指令，需要跟標籤一起使用．

- `blog in blogs`: 前面的 `blog` 是一個臨時變量，用於遍歷使用． 

後面的`blogs` 是http 請求成功後，　`this.blogs = ... ` 這個變量．

同時，這個`this.blogs` 是聲明於　`data`鉤子方法中．

- \{\{blog.title}} 用來顯示每個blog.title的值

## 如何發起post請求？

跟get特別類似，就是第二個參數是　請求的body.

在　vue的配置文件中　（例如　webpack項目的　src/main.js 中）增加下面一句：

```
import VueResource from 'vue-resource';
Vue.use(VueResource);
....

//增加下面這句：
Vue.http.options.emulateJSON = true;
```

上面這句的目的，是爲了能夠讓發出的post請求不會被瀏覽器轉換成option　請求．

然後就可以按照下面的代碼發送請求了：

```
this.$http.post('api/interface/blogs/all', {title: '', blog_body: ''})
	.then((response) => {
		...
	}, (response) => {
		...
	});
```

在本書的 《表單的提交》 章節中，會對 http POST 的發送有個實際的例子，看起來會更加明白。

關於發送http請求的更多內容，請看： 官方文檔：https://github.com/pagekit/vue-resource