# Vue2 to WeChat Mini Program Converter Skill

A Trae IDE Skill that converts Vue2 Single File Components (.vue) into native WeChat Mini Program files (.wxml, .wxss, .js, .json).

---

# Vue2 转原生微信小程序转换器 Skill

Trae IDE 技能，将 Vue2 单文件组件（.vue）转换为原生微信小程序文件（.wxml、.wxss、.js、.json）。

## What It Does / 功能说明

This skill provides comprehensive conversion rules and examples for migrating Vue2 projects to native WeChat Mini Programs, covering:

- **Template → WXML**: Directives, data binding, event binding, refs, slots, HTML tag mapping
- **Script → JS**: Page/Component definitions, `setData` reactivity, computed properties, watchers, lifecycle hooks, props, events, mixins
- **Style → WXSS**: Unit conversion (px→rpx), scoped styles, CSS selector limitations, external classes
- **Config & Store**: JSON configuration, navigation/routing, network requests, local storage, Vuex→global store

本技能提供将 Vue2 项目迁移到原生微信小程序的完整转换规则和示例，涵盖：

- **模板 → WXML**：指令、数据绑定、事件绑定、模板引用、插槽、HTML 标签映射
- **脚本 → JS**：页面/组件定义、`setData` 响应式、计算属性、侦听器、生命周期钩子、Props、事件触发、混入
- **样式 → WXSS**：单位转换（px→rpx）、样式作用域、CSS 选择器限制、外部样式类
- **配置与状态管理**：JSON 配置、导航路由、网络请求、本地存储、Vuex→全局 Store

## Project Structure / 项目结构

```
Vue2ToWxMiniProgram/
├── SKILL.md                          # Skill 主文件（核心原则、工作流、Reference 索引）
├── README.md                         # 项目说明文档
└── references/                       # 按需加载的详细转换参考
    ├── template.md                   # 模板 → WXML 转换规则
    ├── script.md                     # 脚本 → JS 转换规则
    ├── style.md                      # 样式 → WXSS 转换规则
    └── config-and-store.md           # 配置、路由、请求与状态管理转换规则
```

## How It Works / 工作原理

The main `SKILL.md` file loads with core conversion principles (7 key rules), workflow, and common pitfalls. Detailed reference files are loaded on-demand based on the specific conversion task, reducing token consumption.

`SKILL.md` 主文件加载核心转换原则（7 条关键规则）、工作流和常见陷阱。详细的参考文件根据具体转换任务按需加载，减少 token 消耗。

| Reference File / 参考文件 | Content / 内容 | When to Load / 何时加载 |
|---------------------------|---------------|------------------------|
| `references/template.md` | Template→WXML rules / 模板转换规则 | Converting `<template>` / 转换模板时 |
| `references/script.md` | Script→JS rules / 脚本转换规则 | Converting `<script>` / 转换脚本时 |
| `references/style.md` | Style→WXSS rules / 样式转换规则 | Converting `<style>` / 转换样式时 |
| `references/config-and-store.md` | Config, routing, API, store / 配置路由请求状态管理 | Converting config/routing/store / 转换配置时 |
