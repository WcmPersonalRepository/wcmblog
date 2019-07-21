# 移动端兼容性问题

## ISO input 自动填充背景黄色

问题描述：iOS 部分 Safari 浏览器版本从外部复制粘贴内容到 input 输入框中，会出现黄色背景。原因是复制粘贴触发了浏览器的自动填充伪类：-webkit-autofill

解决方案 1 ：用阴影填充

```css
input:-webkit-autofill,
textarea:-webkit-autofill,
select:-webkit-autofill {
  -webkit-box-shadow: 0 0 0 1000px white inset;
}
```

解决方案 2 ：修改成透明色

```css
input[type='text'] {
  border: 1px solid #333333 !important;
  padding: 0px 12px 0px 32px;
  background: transparent !important;
}
input[type='password'] {
  border: 1px solid #333333 !important;
  padding: 0px 12px 0px 32px;
  background: transparent !important;
}

@-webkit-keyframes autofill {
  to {
    color: #666;
    background: transparent;
  }
}

input:-webkit-autofill {
  animation-name: autofill !important;
  animation-fill-mode: both !important;
}

input:-webkit-autofill,
input:-webkit-autofill:hover,
input:-webkit-autofill:focus {
  background-clip: content-box !important;
}
```

## ISO new Date()问题

问题描述：苹果的 Safari 浏览器中 new Date()创建实例是传入类似 yyyy-MM-dd hh:mm:ss 时间字符串无论传入什么时间，都会得到 1970 年 1 月 1 日或 Invalid Date 的情况。原因是在苹果的 Safari 浏览器中想用时间字符串创建时间日期分隔符必须为“/”，即类似 yyy/MM/dd hh:mm:ss 的时间字符串格式

解决方案：

```js
new Date('2019/02/02 08:08:08')
```

## 移动端输入框被遮挡问题

```js
setTimeout(function() {
  document.body.scrollTop = document.body.scrollHeight
}, 300)
```

## iOS 浏览器 video 自动全屏

问题描述：在 iOS 浏览器中的 video 元素，会出现自动全屏的情况

::: tip 官网描述
You must set this property to play inline video. Set this property to true to play videos inline. Set this property to false to use the native full-screen controller. When adding a video element to a HTML document on the iPhone, you must also include the playsinline attribute.
:::

::: tip 官网描述
Apps created before iOS 10.0 must use the webkit-playsinline attribute.
:::

解决方案：

```html
<video playsinline="true" webkit-playsinline="true"></video>
```

## ios 键盘弹起把页面弹起回落

问题描述：在 iOS 浏览器中当输入框键盘弹起输入完成后，页面无法自动回落

解决方案：

```js
document.body.scrollTop && (document.body.scrollTop = 0)
```

## 微信浏览器中保存长按保存解决方案

问题描述：微信浏览器中可能会出现长按图片不弹起保存图片菜单的情况

解决方案：给图片添加一下样式

```css
-webkit-touch-callout: none;
```
