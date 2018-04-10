# cursor-vcode
web端实现app中常见的验证码输入特效
**Web 端 实现 app “输入验证码 ”的效果**

![preview-qr.png](https://user-gold-cdn.xitu.io/2018/4/10/162ae88af6820e3b?w=210&h=211&f=png&s=6089 '二维码预览')
[https://github.com/useryangtao/cursor-vcode](https://github.com/useryangtao/cursor-vcode)


## 起因 _（只是因为在微信上多唠了两句...）_

前两天跟一个哥们唠嗑，讨论怎么实现uber, 滴滴的验证码输入的效果。

![](https://user-gold-cdn.xitu.io/2018/4/10/162adf0e9812625e?w=320&h=391&f=png&s=17481)



## 初构思
**方案1:** 

有打算一个input然后两条线段之间用白背景的线段遮住实现，用letter-spacing控制字间距。

但是控制数字间距吃力点，效果不佳便放弃。

**方案2:**


使用label标签做显示验证码的框，

然后每个label for属性指向同一个 id 为`vcode` 的input，

为了能够出发input焦点， 将input 改透明度样式隐藏，

这样就实现了 点击label触发 input焦点，调用键盘。


## 实现 _（后来~ 终于在眼泪中实现... * *）_

最后尝试选择了 **方案2** 

### 实现效果

![](https://user-gold-cdn.xitu.io/2018/4/10/162ae7b47a9a186e?w=375&h=803&f=gif&s=1868823)


**觉得光标效果挺有意思的，便用 `animate` 搞了下**

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

(vue好用不上火，哈哈哈~ 😂 😂 😂)

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

#### 预览
![preview-qr.png](https://user-gold-cdn.xitu.io/2018/4/10/162ae88af6820e3b?w=210&h=211&f=png&s=6089 '二维码预览')


#### github地址:

[https://github.com/useryangtao/cursor-vcode](https://github.com/useryangtao/cursor-vcode)

如果觉得可以，老铁评论走一波，🤙🤙🤙