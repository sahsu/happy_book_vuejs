# 視圖中的渲染

前面我們介紹了項目的運行（hello world), 文件夾的結構，以及index.html中的內容是如何一點點的渲染出來的。下面，我們來
學習下Vuejs中對於視圖的操作。

## 渲染某個變量

假定我們定義了一個變量：　

```
<script>
export default {
  data () {
    return {
      my_value: '默認值',
    }
  },
}
</script>

```

我們就可以這樣來顯示它：　

```
<div>{{my_value}}</div>
```

完整代碼如下： （ `src/components/Hello.vue`) 

```
<template>
  <div>
    {{message}}
  </div>
</template>

<script>
export default {
  data () {
    return {
      message: '你好Vue! 本消息來自於變量'
    }
  }
}
</script>

<style>
</style>
```

可以看到，上面的代碼顯示定義了 `message` 這個變量， 然後把它在 `<h1> {{ message }} </h1>` 中顯示出來。 

打開瀏覽器  http://localhost:8080/#/say_hi_from_variable , 就可以看到下圖所示： 

![say_hi_from_variable](./images/vue_render_page_variable.png)

## 方法的聲明和調用

聲明一個方法：　show_my_value
```
<script>
export default {
  data () {
    return {
      my_value: '默認值',
    }
  },
  methods: {
    show_my_value: function(){
      // 注意下面的　this.my_value, 要用到this這個關鍵字．
      alert('my_value: ' + this.my_value);
    },
  }
}
</script>
```

調用上面的方法

```
<template>
  <div>
    <input type='button' @click="show_my_value()" value='...'/>
  </div>
</template>
```

對於有參數的方法，直接傳遞參數就好了．　例如:

```
<template>
  <div>
    <input type='button' @click="say_hi('Jim')" value='...'/>
  </div>
</template>
<script>
export default {
  data () {
    return {
      my_value: '默認值',
    }
  },
  methods: {
    say_hi: function(name){
      alert('hi, ' + name)
    },
  }
}
</script>
```

上面代碼中，

```
<input type='button' @click="say_hi('Jim')" value='...'/>
```

就會調用 `say_hi` 這個方法 , 傳入參數　'Jim'。

## 事件處理：　v-on

我們看到，很多時候，　`@click` 等同於 `v-on:click`, 下面兩個是一樣的：　

```
<input type='button' @click="say_hi('Jim')" value='...'/>
<input type='button' v-on:click="say_hi('Jim')" value='...'/>
```
