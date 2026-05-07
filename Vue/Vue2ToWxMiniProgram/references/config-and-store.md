# 配置、路由、请求与状态管理转换参考

## JSON 配置

每个页面/组件都需要一个 `.json` 文件：

### 页面 JSON

```json
{
  "navigationBarTitleText": "页面标题",
  "usingComponents": {
    "custom-comp": "/components/custom-comp/index"
  }
}
```

### 组件 JSON

```json
{
  "component": true,
  "usingComponents": {}
}
```

### 应用 JSON（替代路由配置）

```json
{
  "pages": [
    "pages/home/index",
    "pages/detail/index"
  ],
  "tabBar": {
    "list": [
      { "pagePath": "pages/home/index", "text": "首页" },
      { "pagePath": "pages/profile/index", "text": "我的" }
    ]
  },
  "window": {
    "navigationBarTitleText": "我的应用",
    "navigationBarBackgroundColor": "#ffffff"
  }
}
```

## 导航 / 路由

| Vue2（vue-router） | 微信小程序 |
|--------------------|-----------|
| `this.$router.push('/detail')` | `wx.navigateTo({ url: '/pages/detail/index' })` |
| `this.$router.replace('/home')` | `wx.redirectTo({ url: '/pages/home/index' })` |
| `this.$router.go(-1)` | `wx.navigateBack({ delta: 1 })` |
| `this.$router.push('/tab/home')` | `wx.switchTab({ url: '/pages/home/index' })` |
| 路由参数 `this.$route.params.id` | `onLoad(options) { options.id }` |
| 查询参数 `this.$route.query.key` | `onLoad(options) { options.key }` |

## 网络请求

```javascript
// Vue2（axios）
axios.get('/api/data', { params: { id: 1 } })
  .then(res => { this.data = res.data })

// 小程序
wx.request({
  url: 'https://example.com/api/data',
  data: { id: 1 },
  method: 'GET',
  success: (res) => {
    this.setData({ data: res.data })
  }
})
```

## 本地存储

| Vue2 | 微信小程序 |
|------|-----------|
| `localStorage.setItem(key, val)` | `wx.setStorageSync(key, val)` |
| `localStorage.getItem(key)` | `wx.getStorageSync(key)` |
| `localStorage.removeItem(key)` | `wx.removeStorageSync(key)` |
| `localStorage.clear()` | `wx.clearStorageSync()` |

异步版本：`wx.setStorage`、`wx.getStorage`、`wx.removeStorage`、`wx.clearStorage`。

## Vuex → 小程序状态管理

### 简单模式（全局 store）

```javascript
// store.js
const store = {
  _state: { user: null, token: '' },
  _listeners: [],
  getState() { return this._state },
  setState(partial) {
    Object.assign(this._state, partial)
    this._listeners.forEach(fn => fn(this._state))
  },
  subscribe(fn) {
    this._listeners.push(fn)
    return () => {
      this._listeners = this._listeners.filter(l => l !== fn)
    }
  }
}

export default store
```

### 在组件中使用

```javascript
const app = getApp()

Component({
  attached() {
    const store = app.store
    this.setData({ ...store.getState() })
    this._unsubscribe = store.subscribe((state) => {
      this.setData({ ...state })
    })
  },
  detached() {
    this._unsubscribe()
  }
})
```
