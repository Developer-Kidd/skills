# 脚本 → JS 转换参考

## 页面定义

Vue2 页面组件：

```javascript
export default {
  data() {
    return { count: 0 }
  },
  methods: {
    increment() { this.count++ }
  }
}
```

小程序页面：

```javascript
Page({
  data: {
    count: 0
  },
  increment() {
    this.setData({ count: this.data.count + 1 })
  }
})
```

## 组件定义

Vue2 组件：

```javascript
export default {
  props: { title: String },
  data() { return { count: 0 } },
  computed: { double() { return this.count * 2 } },
  methods: { increment() { this.count++ } }
}
```

小程序组件：

```javascript
Component({
  properties: {
    title: { type: String, value: '' }
  },
  data: { count: 0 },
  observers: {
    count(val) {
      this.setData({ double: val * 2 })
    }
  },
  methods: {
    increment() {
      this.setData({ count: this.data.count + 1 })
    }
  }
})
```

## 响应式：`this.xxx = val` → `this.setData({ xxx: val })`

这是最关键的转换。Vue2 中直接赋值即可触发响应式更新，小程序中必须使用 `setData`：

```javascript
// Vue2
this.count = 1
this.list.push(item)
this.user.name = 'Tom'
this.visible = true

// 小程序
this.setData({ count: 1 })
this.setData({ list: this.data.list.concat(item) })
this.setData({ 'user.name': 'Tom' })
this.setData({ visible: true })
```

数组变更操作，需使用不可变模式：

| Vue2 变更方法 | 小程序等效写法 |
|---------------|--------------|
| `this.list.push(item)` | `this.setData({ list: this.data.list.concat(item) })` |
| `this.list.pop()` | `this.setData({ list: this.data.list.slice(0, -1) })` |
| `this.list.splice(i, 1)` | `this.setData({ list: this.data.list.filter((_, idx) => idx !== i) })` |
| `this.list[i] = val` | `this.setData({ ['list[' + i + ']']: val })` |
| `this.list.splice(i, 0, item)` | 创建新数组插入元素后，再 `setData` |

## 计算属性 → observers + data

Vue2 计算属性必须转换为通过 `observers` 更新的 `data` 字段：

```javascript
// Vue2
computed: {
  fullName() { return this.firstName + ' ' + this.lastName }
}

// 小程序
data: { fullName: '' },
observers: {
  'firstName, lastName': function(firstName, lastName) {
    this.setData({ fullName: firstName + ' ' + lastName })
  }
}
```

仅依赖 props 的计算属性：

```javascript
// Vue2
computed: {
  formattedTitle() { return this.title.toUpperCase() }
}

// 小程序
observers: {
  title(val) {
    this.setData({ formattedTitle: val.toUpperCase() })
  }
}
```

## 侦听器 → observers

```javascript
// Vue2
watch: {
  keyword(val) { this.search(val) }
}

// 小程序
observers: {
  keyword(val) {
    this.search(val)
  }
}
```

深度侦听：

```javascript
// Vue2
watch: {
  'form.name': { handler(val) { ... }, immediate: true }
}

// 小程序
observers: {
  'form.name': function(val) { ... }
}
```

注意：小程序 `observers` 不支持 `immediate: true`。需要在 `attached` 或 `created` 生命周期中手动执行初始逻辑。

## 生命周期钩子映射

### 页面生命周期

| Vue2 | 微信小程序 |
|------|-----------|
| `created` | `onLoad` |
| `mounted` | `onReady` |
| `activated` | `onShow` |
| `deactivated` | `onHide` |
| `beforeDestroy` | `onUnload` |

### 组件生命周期

| Vue2 | 微信小程序 |
|------|-----------|
| `beforeCreate` | 无对应（使用 `created`） |
| `created` | `created`（尚不能使用 `setData`） |
| `beforeMount` | 无对应 |
| `mounted` | `attached`（可以使用 `setData`） |
| `beforeUpdate` | 无对应 |
| `updated` | 无对应 |
| `beforeDestroy` | `detached` |
| `destroyed` | `detached` |

## Props → properties

```javascript
// Vue2
props: {
  title: { type: String, default: '' },
  count: { type: Number, default: 0 },
  items: { type: Array, default: () => [] },
  isActive: { type: Boolean, default: false }
}

// 小程序
properties: {
  title: { type: String, value: '' },
  count: { type: Number, value: 0 },
  items: { type: Array, value: [] },
  isActive: { type: Boolean, value: false }
}
```

类型映射：`String→String`、`Number→Number`、`Boolean→Boolean`、`Array→Array`、`Object→Object`。

## 事件触发 → triggerEvent

```javascript
// Vue2
this.$emit('change', value)
this.$emit('update:visible', false)

// 小程序
this.triggerEvent('change', { value })
this.triggerEvent('updatevisible', { value: false })
```

在 WXML 中，父组件监听：

```xml
<!-- Vue2 -->
<Child @change="onChange" />

<!-- 小程序 -->
<child bind:change="onChange" />
```

## 混入 → behaviors

```javascript
// Vue2
const myMixin = {
  data() { return { shared: true } },
  methods: { commonMethod() {} }
}

// 小程序
const myBehavior = Behavior({
  data: { shared: true },
  methods: { commonMethod() {} }
})
```

使用方式：

```javascript
// Vue2
export default { mixins: [myMixin] }

// 小程序
Component({ behaviors: [myBehavior] })
```
