# Webpack下的Vuejs

所有的Vuejs 項目，都是在Webpack的框架下進行開發的。 

可以說， vue-cli 直接把webpack做了集成。 我們在開發的時候，一邊享受着飛一般的開發速度，一邊體驗着webpack帶來的便利。

在本章，會介紹如何使用兩者進行開發的基本知識。

## 學習過程

我們在學習任何一種框架的時候，都是按照循序漸進的順序來的：

1. 安裝
2. 新建一個頁面
3. 做一些簡單的變量的渲染  
4. 實現頁面的跳轉  (路由)
5. 實現頁面間的參數的傳遞 (路由)
6. 實現真實的http請求（訪問接口)
7. 提交表單       
8. 使用一些技巧來讓代碼層次化（組件）

按照我之前在惠普，聯通，移動等公司的講課經驗，只要用一天的時間把本章內容學會，就可以上手開發Vuejs項目了。 

同學們只要手頭有個電腦，看一個章節，就跟着老師來敲一些代碼，就肯定可以在一天內把這些基本的內容消化完。 

## 可以跳過的章節

對於有一定node基礎的同學，可以跳過 "NVM的安裝"

對於使用Linux/Mac的 同學，可以跳過 "Git Bash的安裝"

## 簡寫說明

由於本章節是 Webpack + Vuejs 下的實戰開發，所以統一使用 "Vuejs" 來代替冗長的 "Webpack + Vuejs".  

例如，

```
在Vuejs中創建頁面需要兩步：

1. 新建路由
2. 新建vue頁面
```

中的 "Vuejs", 指的就是在 "Webpack + Vuejs"的項目中。 而不是 "原始Vuejs".

## 例子

本章節中的所有例子，由於都需要跟webpack相結合，所以我把它單獨拉出來成爲一個項目：  https://github.com/sg552/vue_js_lesson_demo  

大家可以下載後直接運行。 

```
$ git clone https://github.com/sg552/vue_js_lesson_demo.git
$ npm install -v
$ npm run dev
``` 

然後就可以在  http://localhost:8080/#/ 中看到效果，如下圖所示：

![lesson demo](./images/vue_lesson_demo.png)