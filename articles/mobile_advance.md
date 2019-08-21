# 移动端优化

## 键盘显示搜索按钮

```js
<form action="javascript:return true">
    <input type="search" :placeholder="请输入" autofocus @keyup.13="enterClick" v-model="value"/>
</form>
```

## 修改 search 的默认样式

```css
input[type='search'] {
  -webkit-appearance: none;
  // 也可以去除加上border: 0;之类的 根据设计图来
}
input::-webkit-search-cancel-button {
  display: none;
} // 关闭的按钮
```
