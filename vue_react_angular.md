# 知名的單頁應用(SPA) 框架對比

在學習Vuejs之前，我們要知道爲什麼要學習它。 

目前市面上最知名的SPA框架，包括： Vuejs, React, Angular. 我們依次來看一下~


## Angular

作爲SPA的老大哥，源於Google, 在過去若干年發揮了巨大的價值。 現在的版本是4.0 

它的優點特點是： 

1. 業內的第一個SPA框架
2. 實現了前端的MVC解耦
3. 雙向綁定。  model層的數據發生變化會直接影響View. 反之亦然。

缺點也很明顯：  

1. 難學, 難用，
2. Angular 1.x 的文檔很爛。 2.0 稍微好一些。 

我個人曾經在13年7月做項目用到它，當時我還服務於優酷. 

angular 1. 它的文檔: directive. 被無數人(老外)吐槽: 看不懂. 'The worst document that I've ever read'.  3 個月沒變. (記得當時的頁面的很多留言都是一年以前的)

文檔不全，沒有示例代碼。很多東西調試起來也沒有專門的工具。 

另外，想使用第三方組件的話，需要單獨爲Angular做適配。 例如 jquery-upload 這個大名鼎鼎的前端上傳文件的組件。 用起來非常不好用。

總之，功能很全面，但是由於學習曲線過於陡峭，上手很慢，維護起來麻煩。

所以現在在論壇上的口風開始下降。 

官方網站： https://github.com/angular   截止到 2018.6月下旬， github 關注數 3.8萬

## React

由Facebook推出的SPA框架。 是與Vuejs並列的兩大框架之一。 宣稱的特點“ Learn once, write anywhere” 很吸引人。 

優點：

1. 使用js 一種語言就可以寫前端(h5, app) + 後端. 
2. ReactNative 可以直接運行在手機端，性能很棒，接近於原生App.  並且可以熱更新， 免去了手機端App每次都要重新下載安裝才行。
3. 周邊組件很多。

缺點：

1.html代碼(<div>這樣的標籤) 需要寫在js 文件中, 例如：

```
class HelloMessage extends React.Component {
  render() {
    return (
      <div>
        Hello {this.props.name}
      </div>
    );
  }
}

ReactDOM.render(
  <HelloMessage name="Taylor" />,
  mountNode
);
```

這樣的“多語言混合式編程” 引起的最大特點，是代碼難以理解，非常難以開發和調試。  

2.把前後端代碼寫在一起的風格

`````````````
//前端代碼

....


// 後端代碼
....

`````````````

所有傳統web框架過的人( java, rails, php) 都覺得奇怪. 

其他表現尚可。 學習難度低於 Angular, 但是高於Vuejs.  

官方網站： https://github.com/facebook/react  截止2018年6月底，關注人數是10.4萬。 

## Vuejs

Vuejs(讀音同 "View") 是一個MVVM (Model - View - ModelView) 的SPA框架。 

- View: 視圖
- Model: 數據
- ModelView: 連接View與Model 的紐帶

Vuejs一經推出，就獲得了各大社區的好評。 幾乎是一邊倒的聲音。 它的優點是：

1.簡單好學, 好用.

- Angular: 學習二週到4周
- React:  學習兩週
- Vuejs:  三天到一週.

做的事兒都一樣. 

2.Angular, React具備的功能， Vuejs都具備。 (React Native 除外)

3.作者尤雨溪（ 個人網站： http://evanyou.me ） 是中國人. 目前在美國google工作

所以Vuejs 在2014年2月推出的時候，核心文檔就具備了兩種語言:  中文、英文. 這對於母語是漢語的國人來說意義重大。 可以非常快的上手。 

官方網站 https://github.com/vuejs/vue  截止2018年6月底，關注人數10.5萬。 是三大框架中的最高者。

## 爲什麼用vue , 不用 react, angular?

我們在評價一個技術的時候，一個最簡單的辦法，就是看它有多“火”。 這個體現在Github的 stars 數目上。

截止到 2018年6月底， 三個項目的關注數分別是：

- Angular: 3.8萬
- React: 10.4萬
- Vuejs: 10.5萬

可以看出， Vuejs 第一。

其次，我們看一下 stars 的增長趨勢。

根據統計，（http://www.timqian.com/star-history/#facebook/react&angular/angular&vuejs/vue ），截止到2018-6-23, 
github star曲線如下圖所示，可以看出， Vuejs 的增長勢頭一直是最高的, React居中， Angular最低: 

![關注度增長趨勢](./images/github_stars_compare.png)

第三，Vuejs的作者就是中國人，官方文檔之一就是中文。 （地址： http://cn.vuejs.org ）  這個對於普遍英語水平不好的國人程序員來說，可以非常快的上手，是個巨大的優勢。

參考文章：

https://cn.vuejs.org/v2/guide/comparison.html

https://www.quora.com/How-does-Vue-js-compare-to-React-js

