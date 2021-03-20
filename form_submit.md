# 表單的提交

大家要切記這一點：　在任何 Single Page App中，js代碼都不會產生. 一個傳統意義的form表單提交！（這會引起整個頁面的刷新）

所以，我們往往用事件來實現．（桌面開發思維）

例如，在遠程有個接口，可以接受別人的留言：

- URL: http://siwei.me/interface/blogs/add_comment
- 參數：　`content`:  留言的內容．
- 請求方式： `POST`
- 返回結果：

```
{"result":"ok","content":"(留言的內容)"}
```

我們可以先用POSTMAN來確認一下:

![postman對於接口的調用](./images/postman_interface_add_comment.png)

例如，下面的代碼，就是把輸入的表單，提交到我們的後臺．

新增加一個文件: `/src/components/FormSubmit.vue`, 內容如下： 

```
<template>
  <div>
    <textarea v-model='content'>
    </textarea>
    <br/>
    <input type='button' @click='submit' value='留言'/>
  </div>
</template>
<script>

export default {
  data () {
    return {
      content: ''
    }
  },
  methods: {
    submit: function(){
      this.$http.post('/api/interface/blogs/add_comment',
        {
          content: this.content
        }
      )
      .then((response) => {
          alert("提交成功!, 剛纔提交的內容是：" + response.body.content)
        },
        (response) => {
          alert("出錯了")
        }
      )
    }
  }
}
</script>
```

上面的代碼中： 

```
    <textarea v-model='content'>
    </textarea>
```

就是待輸入的表單項 。 

```    
<input type='button' @click='submit' value='留言'/>
```
則是按鈕，點擊後會觸發提交表單的函數 `submit`. 


```
    submit: function(){
      this.$http.post('/api/interface/blogs/add_comment',
        {
          content: this.content
        }
      )
      .then((response) => {
          alert("提交成功!, 剛纔提交的內容是：" + response.body.content)
        },
        (response) => {
          alert("出錯了")
        }
      )
    }
```

上面的代碼，定義了提交表單的具體函數 `submit`. 

- `this.$http.post` 表示 發起的http 的類型是 post. 

- `post` 函數的第一個參數是 url, 第二個參數是一個json,  `{ content: this.content}` 代表了我們要提交的數據

- `then`函數的處理同 http get 請求


接下來，我們修改路由文件： `src/router/index.js`, 增加內容如下：

```
import FormSubmit from '@/components/FormSubmit'

export default new Router({
  routes: [
    {
      path: '/form_submit',
      name: 'FormSubmit',
      component: FormSubmit
    }
  ]
} )
```

訪問url:   `http://localhost:8080/form_submit` , 輸入一段字符串, 如下圖所示：

![輸入表單](./images/form_submit1.png)

點擊提交按鈕，就可以看到，內容已經提交，並且得到了返回的response, 觸發了 `alert`， 如下圖所示：

![輸入表單](./images/form_submit2.png)

查看一下返回的json： 

```
{"result":"ok","content":"\u7533\u8001\u5e08\uff0c\u6211\u5b66\u4e60\u5230\u4e86 Vuejs\u7684\u8868\u5355\u7684\u63d0\u4ea4\u4e86\u3002"}
```

至此，完成了一個完整的 輸入表單，提交表單的過程。