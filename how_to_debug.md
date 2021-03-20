# 如何Debug

瀏覽器環境下的javascript, 實際上有兩個天生缺陷：

1. 不嚴謹. 不同瀏覽器的js實現上會略有不同. 這個問題在android, ios上也一樣.
2. 不是嚴格意義上的計算編程語言. 有語法漏洞. 例如 "=="

所以, 我們要駕馭好JS語言,就要知道如何有效的Debug.

## 時刻留意 vue server

我們開發時的命令:

```
$ npm run dev
```

會開啓一個"開發服務器", 這個開發服務器的後臺,我們要時刻留意輸出.
有時候我們把代碼寫錯了, 導致Vuejs無法編譯, 前臺就會一片空白, 還沒有任何出錯提示.

![後臺錯誤提示](./images/vuejs_server_error.png)

上面的錯誤提示很好理解,說 "編譯時出現錯誤",也給出了錯誤的詳細位置.

## 看developer tools 提出的日誌

無論是 chrome, safari 還是firefox, 以及  IE 7+ , 都帶有這個工具. 特別好用. 任何時候
頁面空白了,都要首先看它, 而不是問別人: "頁面怎麼不動了?"

由於JS代碼不是特別嚴謹, 所以給出的錯誤提示也都很概括.我們可以做個對比:

- JSP  錯誤可以精確到某行
- PHP  錯誤可以精確到某行
- Rails 錯誤可以精確到某行
- Vuejs, Angular, Titanium 等JS框架: 錯誤可以精確到 "某個文件".

這是由於, 所有的JS框架的表現層, 都是"框架怪胎", 是一種跟js語言環境妥協的代碼.
出了問題很難定位到最底層的根源.

而 JSP, PHP, Rails ERB, 則是 "正常框架", 出了問題可以直接找到最底層.

所以,我們要有一定的經驗來Debug. 來理解錯誤日誌.

例如下圖:

![錯誤提示,來自於dev tool](./images/vuejs_console_error.png)

```
vue.esm.js?65d7:434 [Vue warn]: Property or method "博客詳情頁" is not defined on
the instance but referenced during render. Make sure to declare reactive data
properties in the data option.

found in

---> <Blog> at /workspace/test_vue_0613/src/components/Blog.vue
       <App> at /workspace/test_vue_0613/src/App.vue
         <Root>
```

- `vue.esm.js?65d7:434` 表示錯誤的來源. 這個文件一般人不知道來自於哪裏, 我們暫且認爲它來自於
臨時產生的文件,或者虛擬js機中.
- `Property or method "博客詳情頁" is not defined ...` 這句話提示了錯誤的原因.
- `found in ... <Blog> at ...` 這裏則是調用棧, 可以看出, 文件是從下調用到最上面的. 問題出在最
上面的文件. 但是沒有給出錯誤的行數.




## 查看頁面給出的錯誤提示(來自於dev server)

如下圖:

![錯誤提示,來自於服務器](./images/vue_error_from_page.png)


```
  Error compiling template:

    <div class='logo'>
      <img src='http://siweitech.b0.upaiyun.com//image/570/siwei.me_header.png'/>
    </div>
  <template>
  <script>
  </script>
  - Component template should contain exactly one root element. If you are using v-if
  on multiple elements, use v-else-if to chain them instead.
  - Templates should only be responsible for mapping the state to the UI. Avoid
  placing tags with side-effects in your templates, such as <script>, as they
  will not be parsed.
  - tag <template> has no matching end tag.
```

這裏 `Error compiling template: ` 給出了提示, 錯誤是由於 模板在被編譯時產生的.

下面給出的HTML代碼則是出錯的點.


```
@ ./src/components/Logo.vue 6:2-177
@ ./~/babel-loader/lib!./~/vue-loader/lib/selector.js?type=script&index=0!./src/components/BlogList.vue
@ ./src/components/BlogList.vue
@ ./src/router/index.js
@ ./src/main.js
@ multi ./build/dev-client ./src/main.js
```

這裏是調用棧. 可以看到,`@ ./src/components/Logo.vue 6:2-177`, 所以錯誤在於
Logo.vue, 第六行第二列.
