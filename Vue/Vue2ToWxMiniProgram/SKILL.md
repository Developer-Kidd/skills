---
name: "Vue2ToWxMiniProgram"
description: "将 Vue2 单文件组件转换为原生微信小程序（WXML/WXSS/JS/JSON）。当用户需要将 Vue2 代码迁移到微信小程序或询问 Vue2 到小程序的转换时调用。"
---

# Vue2 转原生微信小程序转换器

将 Vue2 单文件组件（.vue）转换为原生微信小程序文件（.wxml、.wxss、.js、.json）。

## 文件结构转换

一个 `.vue` 文件拆分为四个文件：

```
Component.vue  →  component.wxml   (模板)
                 component.wxss    (样式)
                 component.js      (脚本)
                 component.json    (组件配置)
```

## 核心转换原则

1. **响应式**：`this.xxx = val` → `this.setData({ xxx: val })`，这是最关键的转换
2. **模板指令**：`v-if` → `wx:if`，`v-for` → `wx:for`，`v-show` → `hidden`
3. **事件绑定**：`@click` → `bindtap`，`@click.stop` → `catchtap`
4. **组件标签**：`<div>` → `<view>`，`<span>` → `<text>`，`<img>` → `<image>`
5. **计算属性**：`computed` → `data` + `observers`
6. **生命周期**：页面用 `onLoad/onReady/onShow/onHide/onUnload`，组件用 `created/attached/detached`
7. **组件通信**：`props` → `properties`，`$emit` → `triggerEvent`，`mixins` → `behaviors`

## 转换工作流

1. **分析项目结构** — 识别页面、组件、路由、状态管理
2. **搭建小程序项目** — 创建 `app.js`、`app.json`、`app.wxss`、`project.config.json`
3. **优先转换页面** — 每个 Vue 页面 → 小程序页面目录（4个文件）
4. **转换组件** — 每个 Vue 组件 → 小程序组件目录（4个文件）
5. **转换状态管理** — Vuex → 简单全局 store 模式
6. **转换工具函数** — API 请求、辅助函数、过滤器
7. **转换 app.json** — 路由 → pages 配置、tabBar 配置
8. **测试与迭代** — 逐个验证页面/组件功能

## 常见陷阱

1. **忘记 `setData`** — 直接属性赋值不会更新视图
2. **`setData` 传入大对象** — 只传需要变更的字段，不要传整个 data 对象
3. **数组索引更新** — 使用 `['list[0].name']` 路径语法进行 `setData`
4. **`observers` 初始化触发** — 与 Vue `watch` 不同，小程序 observers 在数据首次设置时也会触发
5. **无 `v-model`** — 必须手动实现双向绑定：`value` + `bindinput`

## Reference 文件

根据转换任务的需要，读取对应的 Reference 文件获取详细转换规则和示例：

| 文件 | 内容 | 何时读取 |
|------|------|---------|
| `references/template.md` | 模板→WXML：指令、数据绑定、事件、refs、插槽、HTML标签映射 | 转换 `<template>` 部分时 |
| `references/script.md` | 脚本→JS：页面/组件定义、setData、computed、watch、生命周期、props、emit、mixins | 转换 `<script>` 部分时 |
| `references/style.md` | 样式→WXSS：单位转换、作用域、选择器限制、外部样式类 | 转换 `<style>` 部分时 |
| `references/config-and-store.md` | JSON配置、导航路由、网络请求、本地存储、Vuex→状态管理 | 转换配置/路由/请求/状态管理时 |
