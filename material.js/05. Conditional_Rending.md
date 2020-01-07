解Vue的循環指令後，這篇我們將介紹也是寫程式時常用的：條件判斷，Vue一樣提供一些指令可以來做條件渲染(Conditional Rendering)，
下面我們來詳細介紹一下。

## 條件渲染指令

***v-show***
用途：根據表達式值的true或false，切換元素的display CSS屬性，含v-show的元素會根據表達式的布林值來達到顯示或消失的效果。
用法：
```html
<div id="app">
    <p v-show="isShow">{{ message }}</p>
</div>
```
```javascript
export default {
  name: 'HelloWorld',
  data () {
    return {
      message: 'Welcome to Your Vue.js App',
      isShow: true
    }
  }
}
```

***v-if*** & ***v-else-if*** & ***v-else***
```v-else-if```為2.1.0版本後才加入。
用途：就像是條件判斷一樣，當條件成立後，才會將某區塊資料綁定並且描繪出來。
用法：

1. 單純使用v-if：
```html
<div id="app">
    <p v-if="isShow">{{ message }}</p>
</div>
```
```javascript
export default {
  name: 'HelloWorld',
  data () {
    return {
      message: 'Welcome to Your Vue.js App',
      isShow: true
    }
  }
}
```
2. v-if & v-else-if & v-else混合使用：
```html
<div id="app">
    <p v-if="type == 'A'">Type is A</p>
    <p v-else-if="type == 'B'">Type is B</p>
    <p v-else>Not A/B</p>
</div>
```
```javascript
export default {
  name: 'HelloWorld',
  data () {
    return {
      type: 'B'
    }
  }
}
```

***v-if*** with ***v-for***
```v-for```優先權高於```v-if```，也就是當這兩個指令同時出現在同一個元素時，```v-if```會隨著```v-for```的循環次數而判斷條件幾次。
範例1：
```html
<div id="app">
    <ul>
        <li v-if="n <= 5" v-for="n in 10">{{ n }}</li>
    </ul>
</div>
```
```javascript
export default {
  name: 'HelloWorld'
}
```
範例2：
```html
<div id="app">
    <ul v-if="animalsArray.length">
        <li v-for="animal in animalsArray">{{ animal }}</li>
    </ul>
</div>
```
```javascript
export default {
  name: 'HelloWorld',
  data() {
    return: {
      animalsArray: [ 'pig', 'dog', 'bird' ]
    }
  }
}
```


