---
name: "JsonToInterface"
description: "将JSON数据转换为TypeScript interface定义。当用户需要将JSON响应或数据结构转换为TypeScript类型定义时调用。"
---

# JSON转TypeScript Interface转换器

将JSON数据转换为TypeScript interface定义，支持复杂嵌套结构、数组、联合类型等。

## 核心功能

1. **基本类型转换**：`string`、`number`、`boolean`、`null`、`undefined`
2. **复杂类型转换**：嵌套对象、数组、联合类型
3. **数组处理**：自动识别数组元素类型，支持混合类型数组
4. **对象处理**：递归处理嵌套对象，生成嵌套interface
5. **命名规范**：自动转换为PascalCase命名，属性名转换为camelCase
6. **可选字段**：自动识别可能为null或undefined的字段
7. **类型安全**：避免使用`any`类型，优先使用`unknown`提高类型安全性

## 转换规则

### 基本类型映射

| JSON值类型 | TypeScript类型 | 示例 |
|------------|----------------|------|
| 字符串     | `string`       | `"hello"` → `string` |
| 数字       | `number`       | `42` → `number` |
| 布尔值     | `boolean`      | `true` → `boolean` |
| null       | `null`         | `null` → `null` |
| undefined  | `undefined`    | `undefined` → `undefined` |

### 数组处理

- 同类型数组：`["a", "b", "c"]` → `string[]`
- 混合类型数组：`[1, "a", true]` → `(number | string | boolean)[]`
- 对象数组：`[{name: "a"}, {name: "b"}]` → `{name: string}[]`
- 数组元素类型：取第一个非 null 元素决定；若数组为空则生成 `unknown[]` 并提示用户补充

### 对象处理

- 简单对象：`{name: "test", age: 18}` → `{name: string; age: number;}`
- 嵌套对象：`{user: {name: "test", profile: {age: 18}}}` → 生成嵌套interface
- 可选字段：如果在同一个对象 key 中出现 `null` 或 `undefined`，标记为 `?`，如 `{name: "test", age: null}` → `{name: string; age?: number | null;}`

### 类型安全

- 避免使用 `any` 类型：对于不确定的类型，使用 `unknown` 代替 `any`，提高类型安全性

### 注释规范

- 为每个字段添加行内注释，说明字段含义
- 注释内容基于字段名和值类型进行推断
- 对于可选字段，在注释中说明其可选性
- 注释使用 `//` 格式，位于字段定义的右侧

## 转换工作流

1. **解析JSON** — 验证JSON格式并解析为JavaScript对象
2. **类型分析** — 递归分析每个字段的类型
3. **生成Interface** — 根据类型分析结果生成TypeScript interface
4. **优化输出** — 应用命名规范、处理可选字段、生成嵌套interface
5. **格式化输出** — 美化代码格式，确保可读性
6. **加入注释** — 在生成的interface中添加行内注释，说明字段含义

## 示例

### 输入JSON
```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "isActive": true,
  "roles": ["admin", "user"],
  "profile": {
    "age": 30,
    "address": {
      "city": "New York",
      "country": "USA"
    }
  },
  "lastLogin": null
}
```

### 输出TypeScript Interface
```typescript
interface User {
  id: number;              // 用户ID
  name: string;            // 用户名
  email: string;           // 邮箱地址
  isActive: boolean;       // 是否活跃
  roles: string[];         // 角色列表
  profile: Profile;        // 用户资料
  lastLogin?: null;        // 最后登录时间（可选）
}

interface Profile {
  age: number;             // 年龄
  address: Address;        // 地址信息
}

interface Address {
  city: string;            // 城市
  country: string;         // 国家
}
```

## Reference 文件

| 文件 | 内容 | 何时读取 |
|------|------|---------|
| `references/conversion-rules.md` | 详细的类型转换规则、命名规范、特殊情况处理 | 需要详细转换规则时 |
| `references/advanced-features.md` | 泛型、联合类型、交叉类型等高级特性支持 | 需要高级类型转换时 |
