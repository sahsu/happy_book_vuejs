# 開發環境的搭建

npm: 3.10.8 (npm > 3.0)

node: v6.9.1 (node > 6.0)

vue: 2.0+

總的來說，只要能安裝上node 和 vuejs就可以。

## 安裝NVM 和 node 

請看對應章節。

## 安裝git

請看對應章節。

## 安裝vuejs

要同時安裝 `vue`和 `vue-cli`這兩個node package.

運行下面這個命令：

```
$ npm install vue vue-cli -g
```

`-g` 表示這個包安裝後可以被全局使用。 

## 運行 vue

創建一個基於 webpack 模板的新項目:

```
$ vue init webpack my-project
```

注意： 我們使用Vue, 都是在 `webpack` 這個大前提下使用的。

安裝依賴:

```
$ cd my-project
$ cnpm install
```

在本地，以默認端口來運行：

```
$ npm run dev
```

然後就可以看到 在本地已經跑起來了。

```
> test_vue_0613@1.0.0 dev /workspace/test_vue_0613
> node build/dev-server.js

> Starting dev server...


 DONE  Compiled successfully in 12611ms   12:16:47 PM

> Listening at http://localhost:8080
```

我們打開 `http://localhost:8080` 就可以看到剛纔創建的項目歡迎頁, 如下圖所示：

![默認歡迎頁](./images/vue_default_hello.jpg)
