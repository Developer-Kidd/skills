# JSON转TypeScript Interface技能

将JSON数据转换为TypeScript interface定义的工具，支持各种复杂数据结构和TypeScript高级特性。

## 功能特性

- ✅ 基本类型转换（string, number, boolean, null, undefined）
- ✅ 复杂嵌套对象支持
- ✅ 数组类型处理（同类型数组、混合类型数组）
- ✅ 可选字段自动识别
- ✅ 命名规范自动转换（PascalCase, camelCase）
- ✅ 联合类型生成
- ✅ 泛型支持
- ✅ 索引签名生成
- ✅ 类型别名和枚举支持
- ✅ 循环引用检测

## 使用方法

### 基本用法

1. 提供JSON数据
2. 自动生成TypeScript interface

### 输入示例

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

### 输出示例

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  isActive: boolean;
  roles: string[];
  profile: Profile;
  lastLogin?: null;
}

interface Profile {
  age: number;
  address: Address;
}

interface Address {
  city: string;
  country: string;
}
```

## 高级用法

### 自定义Interface名称

可以指定根接口的名称：

```
请将以下JSON转换为TypeScript interface，根接口名称为"Product"
{
  "id": 1,
  "name": "Product Name",
  "price": 99.99
}
```

输出：
```typescript
interface Product {
  id: number;
  name: string;
  price: number;
}
```

### 支持混合类型

```json
{
  "data": [1, "string", { "type": "object", "value": true }]
}
```

输出：
```typescript
interface RootInterface {
  data: (number | string | DataItem)[];
}

interface DataItem {
  type: string;
  value: boolean;
}
```

### 支持索引签名

```json
{
  "users": {
    "user1": { "name": "User 1" },
    "user2": { "name": "User 2" }
  }
}
```

输出：
```typescript
interface RootInterface {
  users: {
    [key: string]: User;
  };
}

interface User {
  name: string;
}
```

## 转换规则

### 类型映射

| JSON值类型 | TypeScript类型 |
|------------|----------------|
| "string"   | string         |
| 123         | number         |
| true/false  | boolean        |
| null        | null           |
| undefined   | undefined      |
| []          | Array<>        |
| {}          | interface      |

### 命名规范

- Interface名称：PascalCase
- 属性名称：camelCase
- 嵌套Interface：自动生成描述性名称

### 可选字段

- 值为null或undefined的字段
- 数组中对象的缺失字段

## 注意事项

1. 确保输入的JSON格式正确
2. 对于复杂的JSON结构，可能需要手动调整生成的interface
3. 支持大多数TypeScript高级特性，但某些极复杂的类型可能需要手动定义

## 文件结构

```
JSONToTypeScriptInterface/
├── SKILL.md                    # 技能定义文件
├── README.md                   # 使用说明
└── references/                 # 参考资料
    ├── conversion-rules.md     # 转换规则
    └── advanced-features.md    # 高级特性
```
