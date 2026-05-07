# 模板 → WXML 转换参考

## 指令

| Vue2 | 微信小程序 |
|------|-----------|
| `v-if="cond"` | `wx:if="{{cond}}"` |
| `v-else-if="cond"` | `wx:elif="{{cond}}"` |
| `v-else` | `wx:else` |
| `v-show="cond"` | `hidden="{{!cond}}"` |
| `v-for="item in list"` | `wx:for="{{list}}"`，默认变量名为 `item` |
| `v-for="(item, idx) in list"` | `wx:for="{{list}}" wx:for-index="idx" wx:for-item="item"` |
| `:key="item.id"` | `wx:key="id"` |
| `v-model="val"` | `value="{{val}}" bindinput="onValInput"` |
| `v-html="html"` | `<rich-text nodes="{{html}}"></rich-text>` |
| `v-text="text"` | `{{text}}` |

## 数据绑定

| Vue2 | 微信小程序 |
|------|-----------|
| `:src="url"` | `src="{{url}}"` |
| `:class="cls"` | `class="{{cls}}"` |
| `:class="{active: isActive}"` | `class="{{isActive ? 'active' : ''}}"` |
| `:style="styleObj"` | `style="{{styleStr}}"`（必须是字符串） |
| `:style="{color: red}"` | `style="color: {{red}}"` |
| `:disabled="isDisabled"` | `disabled="{{isDisabled}}"` |

## 事件绑定

| Vue2 | 微信小程序 |
|------|-----------|
| `@click="handler"` | `bindtap="handler"` |
| `@click.stop="handler"` | `catchtap="handler"` |
| `@touchstart="handler"` | `bindtouchstart="handler"` |
| `@touchmove="handler"` | `bindtouchmove="handler"` |
| `@touchend="handler"` | `bindtouchend="handler"` |
| `@submit="handler"` | `bindsubmit="handler"` |
| `@input="handler"` | `bindinput="handler"` |
| `@change="handler"` | `bindchange="handler"` |
| `@scroll="handler"` | `bindscroll="handler"` |
| `@load="handler"` | `bindload="handler"` |

事件处理函数参数差异：Vue2 直接传递事件对象；小程序传递 detail 对象。通过 `e.detail` 而非 `e.target.dataset` 访问事件数据。

## 模板引用

| Vue2 | 微信小程序 |
|------|-----------|
| `ref="myRef"` | `id="myRef"` |
| `this.$refs.myRef` | `this.selectComponent('#myRef')` |
| 多个引用 | `this.selectAllComponents('.myClass')` |

## 插槽

| Vue2 | 微信小程序 |
|------|-----------|
| `<slot></slot>` | `<slot></slot>` |
| `<slot name="header">` | `<slot name="header"></slot>` |
| `#header` 或 `v-slot:header` | 子元素上使用 `slot="header"` 属性 |

## 常用 HTML → 小程序组件映射

| HTML/Vue2 | 微信小程序 |
|-----------|-----------|
| `<div>` | `<view>` |
| `<span>` | `<text>` |
| `<a href="url">` | `<navigator url="url">` |
| `<img :src="url">` | `<image src="{{url}}">` |
| `<input v-model="val">` | `<input value="{{val}}" bindinput="onInput">` |
| `<button @click="fn">` | `<button bindtap="fn">` |
| `<form @submit="fn">` | `<form bindsubmit="fn">` |
| `<ul>/<li>` | `<view wx:for="{{list}}">` |
| `<scroll-view>` | `<scroll-view scroll-y>` |
| `<iframe>` | `<web-view src="{{url}}">` |
