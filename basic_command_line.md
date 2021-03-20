# 基本命令

這個命令都是 `vue-cli` 提供的. 可以認爲是 "webpack + vuejs"的結合。 

## 建立新項目

```
$ vue init webpack my_blog
```

這個命令會建立好一個文件夾。 具體的說明，見《Webpack下的Vuejs項目文件結構》 章節。

## 安裝所有的第三方包

```
$ npm install
```

這個命令是根據 "package.json" 文件中定義的內容，來安裝所有用到的第三方js庫。  所有的文件都會安裝到 "node_module"目錄下。 

着急的同學還可以 輸入 `--verbose` 命令，來查看運行細節：

```
$ npm install --verbose 
```

運行結果如下： 
```
$ npm install --verbose
npm info it worked if it ends with ok
npm verb cli [ 'D:\\nodejs\\node.exe',
npm verb cli   'D:\\nodejs\\node_modules\\npm\\bin\\npm-cli.js',
npm verb cli   'install',
npm verb cli   '--verbose' ]
npm info using npm@6.1.0
npm info using node@v10.5.0
npm verb npm-session d1e752145cbb60ba
npm info lifecycle test_vue_0613@1.0.0~preinstall: test_vue_0613@1.0.0
npm timing stage:loadCurrentTree Completed in 1609ms
npm timing stage:loadIdealTree:cloneCurrentTree Completed in 12ms
npm timing stage:loadIdealTree:loadShrinkwrap Completed in 681ms
...
```

這樣出現問題的時候，就很容易知道卡在哪裏了。

## 在本地運行

使用下列命令：

```
$ npm run dev
```

默認就會在  localhost 的 8080端口，啓動一個小型的web 服務器。 性能可以完全滿足開發使用，還具備代理轉發等功能。 

源代碼發生改變的時候，服務器也會自動重啓。 （偶爾需要手動重啓）

運行的輸入如下所示：

```
dashi@i5-16g MINGW64 /d/workspace/vue_js_lesson_demo (master)
$ npm run dev

> test_vue_0613@1.0.0 dev D:\workspace\vue_js_lesson_demo
> node build/dev-server.js

[HPM] Proxy created: /api  ->  http://siwei.me
[HPM] Proxy rewrite rule created: "^/api" ~> ""
> Starting dev server...
 DONE  Compiled successfully in 2373ms10:55:18

> Listening at http://localhost:8080

 WAIT  Compiling...11:02:14

 DONE  Compiled successfully in 213ms11:02:14

 WAIT  Compiling...11:06:09

 DONE  Compiled successfully in 117ms11:06:09

 WAIT  Compiling...11:06:26

 DONE  Compiled successfully in 103ms11:06:26
 ... 
```

## 打包編譯

```
$ npm run build
```

該命令用來把 "src" 目錄下的所有文件，都打包成 "webpack"的標準文件，供部署使用。 具體的內容見 《打包和部署》 這一章節。