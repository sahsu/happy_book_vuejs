# Vuejs的生命週期

每個 Vuejs 的實例，都會經歷下圖的生命週期。

![生命週期圖](./images/vuejs_lifecycle.png)

可以看出，基本週期是：

1. created       (創建好DOM)
2. mounted       (頁面基本準備好了。)
3. updated       (update 可以理解成人肉手動操作觸發)
4. destroyed    （銷燬)
 
上面步驟中的 1,3,4都是自動觸發。 每個步驟都有對應的 beforeXyz方法

所以, 我們一般使用 `mounted` 作爲頁面初始化時執行的方法


