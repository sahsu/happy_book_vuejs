# 表單項目的綁定

基本上，所有的表單項，無論是`<input/>`, 還是 `<textarea/>`，都需要使用 `v-model`來綁定。

## 表單項: input, textarea, select 等．

使用v-model來綁定　輸入項

```
<input v-model="my_value" style='width: 400px'/>
```

就可以在代碼中獲取到　`this.my_value`的值.


## 表單項的完整例子

```
<template>
  <div>

    input： <input type='text' v-model="input_value"/>,
    輸入的值：{{input_value}}
    <hr/>
    text area： <textarea v-model="textarea_value"></textarea>,
    輸入的值：{{textarea_value}}
    <hr/>
    radio:
    <input type='radio' v-model='radio_value' value='A'/> A,
    <input type='radio' v-model='radio_value' value='B'/> B,
    <input type='radio' v-model='radio_value' value='C'/> C,
    輸入的值：
    {{radio_value}}
    <hr/>
    checkbox:
    <input type='checkbox' v-model='checkbox_value'
      v-bind:true-value='true'
      v-bind:false-value='false'
      /> ,
    輸入的值：
    {{checkbox_value}}
    <hr/>
    select:
    <select v-model='select_value'>
      <option v-for="e in options" v-bind:value="e.value">
        {{e.text}}
      </option>
    </select>
    輸入的值：{{select_value}}

  </div>
</template>

<script>
export default {
  data () {
    return {
      input_value: '',
      textarea_value: '',
      radio_value: '',
      checkbox_value: '',
      select_value: 'C',
      options: [
        {
          text: '紅燒肉', value: 'A'
        },
        {
          text: '囊包肉', value: 'B'
        },
        {
          text: '水煮魚', value: 'C'
        }
      ]
    }
  },
  methods: {
  }
}
</script>
```

對於select 的option, 使用　`v-bind:value`來綁定option的值.


效果如圖：　

![表單組件的效果](./images/vue_form.png)

動圖如下：

![表單組件的效果](./images/vue_form.gif)


## Modifiers （後綴詞）

### .lazy 

可以讓輸入後不會立刻變化， 而是等焦點徹底離開後（觸發 `blur()`事件後）纔會觸發視圖層的值的變化。 

使用方式： 

```
<input type='text' v-model.lazy="input_value"/>
```

這個可以用在某些需要等待用戶輸入完字符串才需要給出反應的情況，例如 “搜索” 。

### .number

強制要求輸入數字

使用方式： 

```
<input type='text' v-model.lazy="input_value" type="number"/>
```

### .trim 

強制對輸入的值進行去掉 前後的空格。

使用方式： 

```
<input type='text' v-model.trim="input_value" />
```