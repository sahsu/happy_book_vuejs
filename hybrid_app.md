# Hybrid App: 混合式App

目前App幾乎是每個互聯網公司的標配。 但是不是每個團隊都具備開發原生app的能力。

於是出現了以 phonegap, titanium, xamarin, react native 等一系列的 混合式App. 

我們可以大概瞭解一下：

- phonegap: 出現的最早，使用了很多H5的技術來實現原生app的功能，例如拍照等。 很有新意。 不過完全沒有實用價值。 響應速度特別慢。 卡頓非常明顯。 

- titanium: 出現的比較早。性能不錯。 跟小程序很類似，使用js,css的類似技術，在不同平臺上只要稍加修改代碼，就可以跨平臺媲美原生App. 缺點是有學習曲線，特別是module很難寫。 
公司被收購了。

- xamarin： 使用 .NET 來實現，跟titanium 很接近。 也有不小的使用羣體。

- react native: 使用js 黑科技，直接生成 Android/IOS, 原理與titanium， xamarin一樣。 但是是目前走的最遠的 混合式開發方式。 性能也很高。 

## 共同的缺點

無論是Titanium, 還是 Xamarin, React native，包括 Weex , 都屬於 使用 js/.net ，然後把代碼改造成 可以被 Android/IOS 的 javascript virtual machine所能接受的情況。

也就是說，對原生的Android/IOS平臺做了封裝。 

這樣的好處，爲兩個不同的編程語言增加了一個統一的編程入口。 

缺點也非常明顯： 做一些普通的事情（展示頁面，點擊按鈕）沒問題， 但是一旦需要用到第三方應用（定製化的地圖，定製化的身份證識別，人臉識別等等），就需要開發人員具備寫“native module”
的能力。 這就要求開發人員 同時精通 Android / IOS （寫這種 native module的要求比單純使用的要求要高得多） 而這背離了 這些技術的初衷 （初衷是讓不太懂 app 編程的人可以快速上手開發）。

## 原生的殼兒 + Webview的開發方式，只適用於IOS.

還有一種，就是原生的殼兒 + Webview的開發方式。

- 原生的殼兒，指的是： 外殼部分完全使用100%的 native app. 
- Webview: 所有的頁面，都是放到了Webview中來展示。 

這個情況，經過我們的實踐，對於 IOS 設備是可行的。 安卓是不行的。

IOS： 機器硬件性能好， 軟件使用Object C 開發，所以用起來效果非常棒，跟native App的體驗是一模一樣的。 每個頁面都可以瞬間打開，而且頁面滑動非常流暢。
但是在開發層面上，幾乎不用考慮頁面的適配。 這個解決了很大的問題。

Android: 機器硬件性能稍差於蘋果， 軟件使用 java 開發，性能比 Object C差。 而且在 Android中， Webview的性能體驗比 IOS的差太多。 每個頁面打開速度是3-10秒。
而且卡頓嚴重。 

我們曾經試着在優化的路子上走了一段時間，發現對於Android + Webview(Vuejs) 的方式，前期開發成本低於原生， 後期維護成本遠高於原生，而且一些問題在混合的架構下, 無解。 

所以，我們的結論： 

開發App,  IOS可以使用混合式開發, Android 務必使用原生開發。 