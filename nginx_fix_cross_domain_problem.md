# 解決域名問題與跨域問題

我們在部署之後, 會發現Vuejs會遇到js 的經典問題: 遠程服務器地址不對,或者跨域問題.

還是用我們本書中的例子爲例。

我們的真正後臺接口是:

http://siwei.me/interface/blogs/all

如下:

![後臺接口](./images/後臺接口.png)

## 域名404 問題

1.使用瀏覽器打開頁面: http://vue_demo.siwei.me/#/blogs , 頁面出錯。

![出錯](./images/api_error_2.png)

2.可以看到，出錯的原因是 404, 打開 "http://vue_demo.siwei.me/api/interface/blogs/all"

![出錯](./images/api_error_3.png)

3.這個問題是由於源代碼中,訪問 `/interface/blogs/all` 這個接口引起的:

在文件`src/components/BlogList.vue` 中，第41行，我們定義了遠程訪問的url:  

```
this.$http.get('/api/interface/blogs/all')...
```

如下圖所示：

![出錯](./images/api_error_4.png)


這是因爲, 在我們開發的時候, vuejs 會通過 `$npm run dev` 命令, 跑起一個 "開發服務器", 這個server中有一個代理, 可以把所有的 以 '/api' 開頭的請求, 如:

```
localhost:8080/api/interface/blogs/all
```

轉發到:

```
siwei.me/interface/blogs/all
```

"開發服務器"的配置如下:

```
proxyTable: {
  '/api': {
    target: 'http://siwei.me',
    changeOrigin: true,
    pathRewrite: {
      '^/api': ''
    }
  }
},
```

所以, 在開發環境下,一切正常.

但是在生產環境中, 發起請求的時候, 就不存在代理服務器,不存在開發服務器（dev server）了,所以會出錯.

（這個問題的解決辦法，我們等下再講。 ）

## 跨域問題

這個問題,是js的經典問題.

比如，有的同學，在解決上面的問題的時候，會問：老師，我們直接把上圖中 41 行的： 

```
this.$http.get('/api/interface/blogs/all')
```

改成： 

```
this.$http.get('http://siwei.me/interface/blogs/all')
```

不就可以了嗎？ 

答案是不可以。 請動手試一下再說話。

一動手，我們就會發現，  如果`vue_demo.siwei.me` 直接訪問`siwei.me`域名下的資源,會報錯.  因爲他們是兩個不同的域名.

代碼形如:

```
this.$http.get('http://siwei.me/api/interface/blogs/all')...
```

我們就會得到報錯： 

```
XMLHttpRequest cannot load http://siwei.me/api/interface/blogs/all.
No 'Access-Control-Allow-Origin' header is present on the requested resource.
Origin 'http://vue_demo.siwei.me' is therefore not allowed access.
```

如下圖所示：

![跨域問題](./images/api_error_cross_domain.png)


## 解決域名問題和跨域問題

其實，上面提到的兩個問題，根源都是一個。 所以解決辦法都是一樣的。 

1.在代碼端, 處理方式不變, 訪問 `/api` + 原接口url。 （無變化）

```
this.$http.get('/api/interface/blogs/all')...
```

2.在開發的時候, 繼續保持vuejs 的代理存在. 配置代碼如下:  （無變化）

```
proxyTable: {
  '/api': {
    target: 'http://siwei.me',
    changeOrigin: true,
    pathRewrite: {
      '^/api': ''
    }
  }
},
```

3.在nginx的配置文件中,加入代理:(詳細說明見代碼中的註釋)  (這個是新增的)

```
  server {
    listen       80;
    server_name  vue_demo.siwei.me;
    client_max_body_size       500m;
    charset utf-8;
    root /opt/app/vue_demo;

    # 第一步,把所有的 mysite.com/api/interface  轉換成:   mysite.com/interface
    location /api {
      rewrite    ^(.*)\/api(.*)$    $1$2;
    }

    # 第二步，　把所有的 mysite.com/interface 的請求，轉發到 siwei.me/interface
    location /interface {
      proxy_pass          http://siwei.me;
    }
  }

```

就可以了．

也就是說，　上面的配置，把　

```
http://vue_demo.siwei.me/api/interface/blogs/all
```

在服務器端的nginx中做了個變換，相當於訪問了：

```
http://siwei.me/interface/blogs/all
```

重啓nginx ,　就會發現生效了．

如下所示：

![解決了跨域問題](./images/api_error_fixed.png)
