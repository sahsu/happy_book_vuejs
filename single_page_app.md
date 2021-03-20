# Single Page App

可以認爲，世界上的Web 應用分兩類： 

1. 傳統Web頁面應用
2. 單頁應用

## 傳統Web頁面

就是我們打開瀏覽器，整個頁面都會打開的應用。

例如，我的個人網站 http://siwei.me , 就是一個最典型的“傳統Web應用”， 每次點擊其中的任意一個鏈接，都會引起頁面的整個刷新。 如下圖所示：

![傳統web頁面](./images/traditional_web_app.png)

從上圖中可以看出， 傳統的頁面，每次打開，都要把頁面中所有的 ".js", ".css", 圖片文件，html文件等等所有的資源，都要加載一遍。  在 上圖中的左下角可以看到，
本次總共加載了 10個請求，(4個css, 2個js, 3個png圖片，還有 1個html文件). 耗時 0.837s.

這個在PC端可以， 但是在手機端會巨慢無比。 特別是安卓機。 

傳統頁面的特點，就是，下面的任何一個操作，都會引起瀏覽器對於整個頁面的刷新：

1. 點擊 <link>
2. 提交 <form>
3. 觸發 `location.href='...'` 這樣的js代碼的時候。

我們看個最傳統的web頁面的例子，如下：

```
<html>
<head>
	<script src="my.js"></script>
	<style src="my.css"></style>
</head>
<body>
	<img src='my.jpg' />
	<p> 你好！ 傳統Web頁面！ </p>
</body>
</html>
```

每個瀏覽器，都會從第一行忠實的解析到最後一行，然後再繼續加載  `my.js`, `my.css`, `my.jpg` 這三個外部資源。

其實很好理解。 這個就是大家最初想象的Web頁面的打開方式。 

## Single Page App

單頁應用，確切的誕生時間不詳，可以肯定的是這個概念在2003年就在論壇上被人討論了。 在2002年四月，誕生了一個網站：http://slashdotslash.com, 就使用了這種思想。

單頁應用的精髓，就是點擊任何鏈接，都不會引起頁面的整體刷新。 只會通過javascript, 來替換頁面的局部內容。

## Ajax : Asynchronous javascript and XML

說到這裏，就不得不 提到另一個概念： Ajax. 

中文可以稱呼爲： js的異步請求。 不過國內統一都叫Ajax.  

Ajax 的概念是： 每次打開新的網頁時， 不要讓頁面整個刷新，而是由"javascript語言", 發起一個 "http 異步請求", 這個“異步請求”的特點，是不會讓當前的網頁卡死。 

用戶可以一邊上下滾動頁面，播放mp4， 一邊等待這個 請求返回數據。 等這個“response” 正常返回後， 由"javascript"控制，來刷新頁面的局部內容。

這樣的好處是： 

1. 大大節省了頁面的整體加載時間。 各種 .js, .css 等資源文件，加載一次就夠了。
2. 節省了帶寬。
3. 同時減輕了客戶端和服務端的負擔。   

在智能手機和 App應用（特別是微信）流行起來之後，大量的網頁都需要在手機端打開，所以Ajax的優勢體現的淋漓盡致。 

雖然Ajax的名字本意是"異步js 與XML"， 不過現在在服務器端返回的數據中，幾乎都是使用了"json"， 拋棄了"XML"

在2005年，國內的程序員論壇開始提及Web 2.0, 其中的 Ajax 技術被人重視。 到了 2006年初，可以說Ajax是前端程序員的加薪利器。 市面上的所有“前端Web程序員”職位 都認爲
Ajax是一個巨大的加分項。

可惜當時jQuery在國內不是很普及，Prototype也沒有流行起來。 我在北京和圈子裏的各大公司的同行們交流時，發現大家用的都是“原生的javascript Ajax”。 這種不借助任何第三方框架
的代碼寫起來非常臃腫，累人，而且考慮到瀏覽器的兼容問題，應用起來很讓人頭大。 

例如，當時的代碼樣子往往是這樣的： 

```
var xmlHttp = false;
/*@cc_on @*/
/*@if (@_jscript_version >= 5)
try {
	xmlHttp = new ActiveXObject("Msxml2.XMLHTTP");
} catch (e) {
	try {
		xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
	} catch (e2) {
		xmlHttp = false;
	}
}
@end @*/
if (!xmlHttp && typeof XMLHttpRequest != 'undefined') {
	xmlHttp = new XMLHttpRequest();
}
```

上面的代碼僅僅是爲了兼容各種瀏覽器。 實際上後面還有幾十行的冗餘代碼，才能正式開始幹活。

到了2008年，國內開始流行 prototype, jquery之後，發起一個Ajax請求的代碼精簡成幾行：

```
jQuery.get('http://some_url?para=1', function(data){
	// 正常代碼
})
```

那時候開始， Ajax在國內變得越來越普及。

### Angular

第一個SPA的知名框架應該是Angular.  由Google在2010年10月推出。 當時的Gmail, google map等應用對於Ajax技術運用到了極致。 而Angular框架一經推出，立刻引燃了單頁應用這個概念。

儘管後來各種SPA框架層出不窮，在2015年以前，Angular 穩坐SPA的頭把交椅。 

### 當下的SPA技術趨勢

成爲了項目開發必不可少的內容， 只要有移動端開發，就會面臨兩個選擇：

1. 要麼做成原生App
2. 要麼做成SPA H5

無論是IOS端還是Android, 都對SPA青睞有加： 

1.打開頁面速度特別快. 

打開傳統頁面，手機端往往需要幾秒，而SPA則在0.x 秒內。

2.耗費的資源更少.  

因爲每次移動端只請求接口數據。和非要不可的圖片資源。

3.對於點擊等操作響應更快。  

對於傳統頁面，手機端的瀏覽器在操作時，點擊按鈕的話，往往會有0.1s 的卡頓，而使用SPA則不會有卡頓的感覺。

4.可以保存瀏覽的歷史和狀態。

不是每一個Ajax框架都可以由這一點內容。 例如大家常用的QQ郵箱，雖然也是頁面的局部刷新， 但是每次打開不同的郵件時， 瀏覽器的網址不會變化。 

而在所有的SPA框架中， 都會有專門處理這個問題的模塊， 往往叫做“router (路由)”.  

例如： 

`http://mail.my.com/#/mail_from_boss_on_0620`  對應 老闆在6月20日發來的郵件。

`http://mail.my.com/#/mail_from_boss_on_0622`  對應 老闆在6月22日發來的郵件。 

在2011, 2012年，各種SPA框架出現了井噴的趨勢，包括 "backbone" , "ember.js" 等幾十上百個不同的框架。 

到了最近幾年， Angular, React, Vuejs是三種最流行的框架。 

