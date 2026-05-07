# 样式 → WXSS 转换参考

## 单位转换

- `px` → `rpx`（响应式像素，750rpx = 屏幕宽度）
- 换算公式：`rpx = px * (750 / 设计稿宽度)`。375px 设计稿：`rpx = px * 2`

## 样式作用域

小程序组件样式默认隔离，使用 `Component()` 构造器时 `options: { styleIsolation: 'isolated' }` 为默认值。无需 `<style scoped>`。

## CSS 选择器限制

- 不能使用 `*` 通配符选择器
- 不能使用属性选择器如 `[data-type]`
- 部分场景不能使用 `:not()` 伪类
- 避免深度选择器（`>>>` 或 `/deep/`）；改用 `externalClasses` 或 `styleIsolation: 'shared'`

## 外部样式类（替代 CSS 属性传递）

```javascript
// Vue2：父组件通过 prop 传递类名
<Child :class="custom-class" />

// 小程序：使用 externalClasses
Component({
  externalClasses: ['custom-class']
})
```

```xml
<!-- 父组件 -->
<child custom-class="red-text" />
```
