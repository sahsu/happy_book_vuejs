# Webpack下的Vuejs項目文件結構

根據前面章節，我們安裝了 webpack, vue-cli, 並運行成功，看到了Vuejs的第一個頁面。 

那麼首先，我們需要對Webpack 下的Vuejs有個全面的瞭解。 

在我們運行了命令後，

```
$ vue init webpack my-project
```

會生成一個嶄新的項目。 它的文件結構如下：

```
▸ build/                // 編譯用到的腳本
▸ config/               // 各種配置
▸ dist/                 // 打包後的文件夾
▸ node_modules/         // node第三方包
▸ src/                  // 源代碼
▸ static/               // 靜態文件, 暫時無用
  index.html            // 最外層文件
  package.json          // node項目配置文件
```

下面我們針對重要的文件和文件夾來依次說明. 

## build 文件夾

這個文件夾中，保留了各種打包腳本。 是不可或缺的，不要隨意修改。

展開後如下：

```
▾ build/
    build.js
    check-versions.js
    dev-client.js
    dev-server.js
    utils.js
    vue-loader.conf.js
    webpack.base.conf.js
    webpack.dev.conf.js
    webpack.prod.conf.js
```

- build.js:

打包使用，不要修改。

- check-versions.js:

檢查npm的版本， 不要修改。

- dev-client.js 和 dev-server.js:

是在開發時使用的服務器腳本。不要修改。（藉助於node這個後端語言，我們在做vuejs開發時，可以通過 `$npm run dev `這個命令，打開一個小的server, 運行vuejs. )

- utils.js

不要修改。 做一些css/sass 等文件的生成。

- vue-loader.conf.js

非常重要的配置文件，不要修改。 內容是用來輔助加載vuejs用到的css source map等內容。

- webpack.base.conf.js
- webpack.dev.conf.js
- webpack.prod.conf.js

這三個都是基本的配置文件。不要修改。

## config 文件夾

跟部署和配置相關。

```
▾ config/
    dev.env.js
    index.js
    prod.env.js
```

- dev.env.js

開發模式下的配置文件，一般不用修改。

- prod.env.js

生產模式下的配置文件，一般不用修改。

- index.js

很重要的文件， 定義了:

- 開發時的端口（默認是8080），
- 圖片文件夾（默認static)，
- 開發模式下的 代理服務器. 我們修改的還是比較多的。

對於這個文件夾的內容，我們會在後續的章節中陸續進行解釋（例如對於某個頁面的渲染過程，如何做代理轉發) 

## dist 文件夾

打包之後的文件所在目錄，如下。

```
▾ dist/
  ▾ static/
    ▾ css/
        app.d41d8cd98f00b204e9800998ecf8427e.css
        app.d41d8cd98f00b204e9800998ecf8427e.css.map
    ▾ js/
        app.c482246388114c3b9cf0.js
        app.c482246388114c3b9cf0.js.map
        manifest.577e472792d533aaaf04.js
        manifest.577e472792d533aaaf04.js.map
        vendor.5f34d51c868c93d3fb31.js
        vendor.5f34d51c868c93d3fb31.js.map
    index.html
```

可以看到，對應的css, js, map, 都在這裏。

請大家注意 文件名中的無意義的字符串，這個是隨機生成的。 目的是爲了讓文件名發生變化 ，方便我們的部署，也方便Nginx服務器重新對該文件做緩存。

- `app.css` : 編譯後 的CSS 文件
- `app.js` : 最核心的js 文件. 幾乎所有的代碼邏輯都會打包到這裏。
- `manifest.js` ： 生成的周邊js 文件
- `vendor.js` ： 生成的vendor.js 文件

另外，每個 .map 文件都非常重要。 可以簡單的認爲，有了 .map 文件，我們的瀏覽器就可以先下載整個的`.js` 文件，然後在後續的操作中“部分加載” 對應的文件。

切記： 這個文件夾不要放到git中。 因爲每次編譯之後，這裏的文件都會發生變化。

## node_modules 文件夾

node項目所用到的第三方包，特別多，特別大。  `$ npm install` 所產生。 所有在 `package.json` 中定義的第三方包都會出現在這裏。

package.json : 

```
  // ...
  "dependencies": {
    "vue": "^2.3.3",
    "vue-resource": "1.3.3",
    "vue-router": "^2.3.1",
    "vuex": "^2.3.1"
  },
  "devDependencies": {
    "autoprefixer": "^6.7.2",
    "babel-core": "^6.22.1",
    "babel-loader": "^6.2.10",
    "babel-plugin-transform-runtime": "^6.22.0",
    "babel-preset-env": "^1.3.2",
    "babel-preset-stage-2": "^6.22.0",
    "babel-register": "^6.22.0",
    //...
  }
  // ...
```

node_modules 文件夾裏面往往會有幾百個文件夾，看起來如下：

![node module folder](./images/node_module_folder.png)

這個文件夾不要放到git中。

## src 文件夾

最最核心的源代碼所在的目錄。 展開後如下所示（不同版本的vue-cli生成的目錄會稍有不同，不過核心都是一樣的）：

```
▾ src/
  ▾ assets/
      logo.png
  ▾ components/
      Book.vue
      BookList.vue
      Hello.vue
  ▾ router/
      index.js
    App.vue
    main.js
```

- assets 文件夾

用到的圖片都可以放在這裏。

- components

用到的"視圖"和"組件"所在的文件夾。（最最核心）

- router/index.js  

路由文件。 定義了各個頁面對應的url. 

- App.vue

如果index.html 是一級頁面模板的話，這個App.vue就是二級頁面模板。
所有的其他vuejs頁面，都作爲該模板的 一部分被渲染出來。

- main.js

沒有實際的業務邏輯，但是爲了支撐整個vuejs框架，存在很必要。
