# 安裝 Vuejs的開發工具：  vuejs devtool

由於Vuejs是一個框架，它是構建於 javascript 的代碼之上，而 javascript 語言的實現並不像其他後端語言（例如 java, python, ruby等） 那樣有着對debug友好的錯誤提示機制。 

這個可以說原因有： 

1. javascript語言是一個非常靈活的語言， 支持 "元編程" ， 而對於任何一個“框架”來說，都會大量的用到 “元編程”的能力。 例如： 

```
var my_code = "var a = 1 + 1; console.info(a) "
eval(some_code)
```

在上面的代碼中，  `eval`方法就是一個最典型的 “元編程”方法（meta programming). 說的通俗些， 元編程就是爲了“讓程序來寫程序”。 元編程能力是評估一個語言是否“高級”的一個重要指標。

在帶來大量好處的同時， 元編程的缺點是：顯示錯誤信息比較麻煩。 往往顯示的錯誤提示或者stack trace 不是特別明朗。 

2. javascript語言的實現機制， 是在各個不同的瀏覽器中實現的。 不同的瀏覽器廠家，都會實現不同的javascript虛擬機（ virtual machine ), 所以，我們往往會發現，一些 javascript的錯誤
往往是不可讀的。例如：

![無法分析的錯誤日誌](./images/debug_vm.png)

上面出錯的原因，就是由於，javascript的代碼中，可能會有不同的"scope", 而該瀏覽器爲每個"scope", 都會分配一個 "virtual machine", 所以，上面打印出來的 stack trace , 很忠實的反應出了問題所在， 就是幾乎沒有可讀性。


所以，爲了方便我們的開發， 各位同學一定要安裝對應的開發組件， 例如：  "Vue.js devtools".  

官方網址：  	https://github.com/vuejs/vue-devtools 

## 安裝步驟

非常簡單。 建議大家使用chrome， 這樣就可以以插件的形式來安裝了。

1. 安裝chrome 

2. 使用科學上網， 打開： https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd

3. 就會看到下圖：

![google商店中的插件](./images/vuejs_devtools_setup.png)

4. 點擊後，會詢問是否安裝，我們點擊"添加擴展程序" 這個按鈕就可以了：

![確認是否安裝](./images/setup_dev_tool.png)

5. 就可以看到，瀏覽器的右上角新增加了灰色的圖標。  安裝成功。

6. 打開  "設置 " -> '更多工具' -> '擴展程序', 就可以看到剛纔安裝的Vuejs了，勾選下面的 "允許訪問文件網址". 如下圖所示：

![允許訪問文件網址](./images/devtool_allow_visit_file.png)


## 使用步驟

在安裝 devtool 之前，我們調試的時候，都是打開瀏覽器的“開發者工具”， 然後看到的類似下圖：

![未安裝devtool時的報錯頁面](./images/devtool_before.png)

安裝好之後，如果你的vuejs 項目是使用 `http` 服務器打開的話， （不是 `file:///...`) 就可以看到了： 

![devltool頁面](./images/devtool_demo.png)
