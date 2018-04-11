# cursor-vcode

**Web 端 实现 app “输入验证码 ”的效果**

![preview-qr.png](https://user-gold-cdn.xitu.io/2018/4/10/162ae88af6820e3b?w=210&h=211&f=png&s=6089 '二维码预览')
![](https://user-gold-cdn.xitu.io/2018/4/10/162adf0e9812625e?w=320&h=391&f=png&s=17481)

## 实现

使用label标签做显示验证码的框，

然后每个label for属性指向同一个 id 为`vcode` 的input，

为了能够触发input焦点， 将input 改透明度样式隐藏，

这样就实现了 点击label触发 input焦点，调用键盘。

### 效果预览

![](https://user-gold-cdn.xitu.io/2018/4/10/162ae7b47a9a186e?w=375&h=803&f=gif&s=1868823)


### 结构
```
<div class="v-code">
<input type="tel" id="vcode" />

<label for="vcode"></label>
<label... * 5(取决于验证码长度 4 or 6)

</div>

```
### 样式
此处省略 ... 详见 [cursor-vcode](https://github.com/useryangtao/cursor-vcode/blob/master/index.html#L29)

### 逻辑

接下来就是实现  让 `label` 与 `input` 的 **value** 对应，这里依赖 `vue`  简单实现下思路


#### html绑定
```
<div id="app">
    <div class="v-code">
      <input
      ref="vcode"
      id="vcode"
      type="tel"
      maxlength="6"
      v-model="code"
      @focus="focused = true"
      @blur="focused = false"
      :disabled="telDisabled">

      <label
        for="vcode"
        class="line"
        v-for="item,index in codeLength"
        :class="{'animated': focused && cursorIndex === index}"
        v-text="codeArr[index]"
        >
      </label>
    </div>
</div>
```

#### javascript
```
var app = new Vue({
    el: '#app',
    data: {
      code: '', // input value
      codeLength: 6, // 验证码长度
      telDisabled: false, // 判断是否输入完
      focused: false // 让input焦点, 决定光标显示所在的位置
    },
    computed: {
      codeArr() {
        // 将 input value 分隔数组 [1, 2, 3]
        return this.code.split('')
      },
      cursorIndex() {
        // 判断code输入长度，光标显示在对应label上
        return this.code.length
      }
    },
    watch: {
      code(newVal) {
        // 限制 非数字
        this.code = newVal.replace(/[^\d]/g,'')
        if (newVal.length > 5) {
        // 禁用 input && 失去焦点 让键盘消失
          this.telDisabled = true
          this.$refs.vcode.blur()
        // submit !!!
          setTimeout(() => {
            alert(`vcode: ${this.code}`)
          }, 500)
        }
      }
    },
    mounted() {}
  })
```

考虑交互，在里面多做了部分细节及限制，慢慢品味~

### 如有雷同，百分百巧合
