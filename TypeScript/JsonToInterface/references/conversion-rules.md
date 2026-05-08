# JSON转TypeScript Interface转换规则

## 1. 类型映射规则

### 1.1 基本类型

| JSON值 | JavaScript类型 | TypeScript类型 |
|--------|---------------|----------------|
| `"string"` | `string` | `string` |
| `123` | `number` | `number` |
| `true`/`false` | `boolean` | `boolean` |
| `null` | `object` | `null` |
| `undefined` | `undefined` | `undefined` |

### 1.2 复杂类型

| JSON结构 | TypeScript类型 |
|----------|----------------|
| `{}` | `interface` |
| `[]` | `array` |
| `[{}, {}]` | `interface[]` |
| `["string", 123]` | `(string  number)[]` |

## 2. 命名规范

### 2.1 Interface命名

- 使用PascalCase命名法
- 默认名称：`RootInterface`
- 嵌套对象：自动生成描述性名称，如`UserProfile`, `AddressInfo`

### 2.2 属性命名

- 使用camelCase命名法
- 保持原有名称的语义
- 特殊字符处理：移除或替换特殊字符，如`user_name` → `userName`

## 3. 可选字段处理

### 3.1 显式可选字段

当字段值为`null`或`undefined`时，标记为可选字段：

```json
{ "name": "test", "age": null }
```
→
```typescript
interface RootInterface {
  name: string;
  age?: number | null;
}
```

### 3.2 隐式可选字段

当数组中对象的字段不统一时，标记为可选字段：

```json
[{ "name": "test1" }, { "name": "test2", "age": 18 }]
```
→
```typescript
interface Item {
  name: string;
  age?: number;
}
```

## 4. 数组处理

### 4.1 同类型数组

```json
["a", "b", "c"]
```
→
```typescript
string[]
```

### 4.2 混合类型数组

```json
[1, "a", true]
```
→
```typescript
(number | string | boolean)[]
```

### 4.3 对象数组

```json
[{ "name": "a" }, { "name": "b" }]
```
→
```typescript
Item[]
```

## 5. 嵌套对象处理

### 5.1 简单嵌套

```json
{ "user": { "name": "test" } }
```
→
```typescript
interface RootInterface {
  user: User;
}

interface User {
  name: string;
}
```

### 5.2 深度嵌套

```json
{ "a": { "b": { "c": "value" } } }
```
→
```typescript
interface RootInterface {
  a: A;
}

interface A {
  b: B;
}

interface B {
  c: string;
}
```

## 6. 特殊情况处理

### 6.1 空值处理

| JSON值 | TypeScript类型 |
|--------|----------------|
| `null` | `null` |
| `undefined` | `undefined` |
| `""` | `string` |
| `0` | `number` |
| `false` | `boolean` |

### 6.2 循环引用

检测到循环引用时，使用接口引用：

```json
{ "name": "test", "parent": { "name": "parent", "children": ["$ref"] } }
```
→
```typescript
interface Node {
  name: string;
  parent?: Node;
  children?: Node[];
}
```

### 6.3 复杂混合类型

```json
{ "data": [1, "a", { "b": true }] }
```
→
```typescript
interface RootInterface {
  data: (number | string | DataItem)[];
}

interface DataItem {
  b: boolean;
}
```
