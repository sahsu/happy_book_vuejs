# 整體項目的搭建

## 爲新項目生成骨架 

創建一個基於 webpack 模板的新項目:

```
$ vue init webpack shop_h5
```

安裝依賴:

```
$ cd shop_h5
$ cnpm install
```

在本地，以默認端口來運行：

```
$ npm run dev
```

然後就可以看到 在本地已經跑起來了。

在這個階段， 我們只需要修改它的標題就可以：


打開  根目錄下的 index.html , 修改<title>字段: 
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>公益愛農</title>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```

用瀏覽器打開後， 就可以看到一個沒有內容的 Vuejs應用已經跑起來了。