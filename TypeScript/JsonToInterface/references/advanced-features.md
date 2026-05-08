# JSON转TypeScript Interface高级特性

## 1. 联合类型支持

### 1.1 基本联合类型

当字段可能有多种类型时，生成联合类型：

```json
{ "value": 123 }
// 或
{ "value": "string" }
```
→
```typescript
interface RootInterface {
  value: number | string;
}
```

### 1.2 复杂联合类型

```json
{ "data": [1, "string", { "type": "object" }] }
```
→
```typescript
interface RootInterface {
  data: (number | string | DataItem)[];
}

interface DataItem {
  type: string;
}
```

## 2. 泛型支持

### 2.1 泛型接口

对于通用数据结构，可以生成泛型接口：

```json
{ "data": { "id": 1, "name": "test" }, "status": "success" }
```
→
```typescript
interface ApiResponse<T> {
  data: T;
  status: string;
}

interface Data {
  id: number;
  name: string;
}
```

### 2.2 泛型数组

```json
{ "items": [{ "id": 1 }, { "id": 2 }] }
```
→
```typescript
interface RootInterface {
  items: Item[];
}

interface Item {
  id: number;
}
```

## 3. 交叉类型支持

### 3.1 交叉类型生成

当对象合并了多个类型的特征时，使用交叉类型：

```json
{ "id": 1, "name": "test", "createdAt": "2023-01-01", "updatedAt": "2023-01-02" }
```
→
```typescript
interface BaseEntity {
  id: number;
  createdAt: string;
  updatedAt: string;
}

interface User {
  name: string;
}

type UserEntity = BaseEntity & User;
```

## 4. 索引签名支持

### 4.1 字符串索引

当对象具有动态键时，生成索引签名：

```json
{ "user1": { "name": "test1" }, "user2": { "name": "test2" } }
```
→
```typescript
interface RootInterface {
  [key: string]: User;
}

interface User {
  name: string;
}
```

### 4.2 数字索引

```json
{ "0": { "name": "test1" }, "1": { "name": "test2" } }
```
→
```typescript
interface RootInterface {
  [key: number]: User;
}

interface User {
  name: string;
}
```

## 5. 类型别名支持

### 5.1 基本类型别名

```json
{ "status": "success" }
```
→
```typescript
type Status = "success" | "error" | "pending";

interface RootInterface {
  status: Status;
}
```

### 5.2 复杂类型别名

```json
{ "coordinates": { "x": 10, "y": 20 } }
```
→
```typescript
type Point = { x: number; y: number };

interface RootInterface {
  coordinates: Point;
}
```

## 6. 枚举类型支持

### 6.1 字符串枚举

当检测到有限的字符串值集合时，生成枚举：

```json
{ "status": "active" }
// 或
{ "status": "inactive" }
```
→
```typescript
enum Status {
  Active = "active",
  Inactive = "inactive"
}

interface RootInterface {
  status: Status;
}
```

### 6.2 数字枚举

```json
{ "type": 1 }
// 或
{ "type": 2 }
```
→
```typescript
enum Type {
  One = 1,
  Two = 2
}

interface RootInterface {
  type: Type;
}
```

## 7. 类型守卫支持

### 7.1 类型守卫生成

对于复杂的联合类型，可以生成类型守卫：

```json
[{ "type": "user", "name": "test" }, { "type": "admin", "permissions": ["read", "write"] }]
```
→
```typescript
interface User {
  type: "user";
  name: string;
}

interface Admin {
  type: "admin";
  permissions: string[];
}

type UserType = User | Admin;

function isUser(obj: UserType): obj is User {
  return obj.type === "user";
}

function isAdmin(obj: UserType): obj is Admin {
  return obj.type === "admin";
}
```

## 8. 高级嵌套结构

### 8.1 递归类型

```json
{ "name": "node1", "children": [{ "name": "node2", "children": [] }] }
```
→
```typescript
interface Node {
  name: string;
  children: Node[];
}
```

### 8.2 条件类型

```json
{ "data": { "type": "string", "value": "test" } }
```
→
```typescript
interface StringData {
  type: "string";
  value: string;
}

interface NumberData {
  type: "number";
  value: number;
}

type DataType = StringData | NumberData;

type ValueType<T extends DataType> = T["type"] extends "string" ? string : number;
```
