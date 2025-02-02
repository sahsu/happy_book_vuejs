# 前言

隨着移動電話的普及，和微信的流行，很多的wap(h5)應用也隨時流行了。 
例如：微店，微站。 以及各個app中包含的h5頁面。 

手機的硬件特點是：

- 硬件設備差。同主頻的手機CPU性能往往是臺式機的10分之一。 （手機的供電跟臺式機設備差的很遠）
- 網絡速度慢。4G 網絡在很多時候下載速度只有幾百K. 可能一個微信中的網頁打開要很久。 

所以，使用傳統的web技術開發的網頁，在手機端的表現往往特別差，因爲傳統技術的特點是：

- 點擊某個鏈接/按鈕，或者提交表單後，web頁面整體刷新。
- js/css 的請求往往很多，過百是很常見的事兒。

每次頁面整體刷新，都要導致瀏覽器重新加載對應的內容。特別卡頓。

另外，加載的內容也很多。很多傳統頁面的css/js 都多達上百個，每次打開頁面，需要發送上百次請求。 如果頁面中包含了websocket 等內容，打開速度就更慢了。

蘋果的機器表現還好，iOS的設備打開Web頁面速度很快，android機則大部分都很慢。 這個是由於手機設備操作系統、軟件以及智能硬件決定的。

所以，單頁應用（Single page app, 簡稱 SPA ) 則體現出了巨大的優勢：

- 頁面是局部刷新的，響應速度快, 不需要每次都加載所有的js/css,
- 前後端分離，前端(手機端) 不受（服務器端的）開發語言限制。

所以越來越多的app採用了 SPA的架構。

如果你的項目要用在h5上，那麼一定使用單頁應用框架。

Angular, React, Vuejs都是很好的框架。

我們在我公司的實際項目中，都使用Vuejs. 效果非常好。 開發速度快，維護效率高。 招人也方便。

本文跟官方文檔不同。是根據實際項目經驗，以 培養新人的角度來寫的，所以會有這樣的特點：

- 我認爲“很少使用的技術”， 略過
- 只講解最常見的知識
- 在章節上，按照入門的難易度從簡單到複雜。

學習的時候，務必要有個開發環境，看一點兒文檔，就動手敲一敲代碼。

否則光看是看不懂的。

本書中的所有示例源代碼，都可以在 https://github.com/sg552/happy_book_vuejs 上找到。