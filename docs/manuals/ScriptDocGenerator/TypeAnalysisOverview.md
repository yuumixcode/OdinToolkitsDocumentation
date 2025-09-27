---
authors: [Yuumix]
date: 2025-09-27
categories:
  - OdinToolkits
tags:
  - ScriptDocGenerator
slug: type-analysis-overview
---
# Type 解析器支持情况总览

## Field 字段解析支持

进行单元测试的字段样式一共有 18 种

### 1.支持多种访问修饰符

单元测试 6 种

``` csharp
int _privateField;
internal int InternalField;
private protected int PrivateProtectedField;
protected int ProtectedField;
protected internal int ProtectedInternalField;
public int PublicField;
```

---

``` markdown
 | 字段完整签名 |
 | :----------|
 | `private int _privateField;` |
 | `internal int InternalField;` |
 | `private protected int PrivateProtectedField;` |
 | `protected int ProtectedField;` |
 | `protected internal int ProtectedInternalField;` |
 | `public int PublicField;` |
```

### 2.支持复合关键字

单元测试 4 种

``` csharp
public const int CONST_FIELD = 42;
public static int StaticField;
public readonly int ReadOnlyField;
public static readonly int StaticReadOnlyField;
```

---

``` markdown
| 字段完整签名 |
| :---------- |
| `public const int CONST_FIELD = 42;` |
| `public static int StaticField;` |
| `public readonly int ReadOnlyField;` |
| `public static readonly int StaticReadOnlyField;` |
```

### 3.支持字段初始值或自定义默认值

单元测试 8 种

``` csharp
class TestClass
{
    // 常量字段
    public const int CONST_FIELD = 100;

    // 静态字段
    public static int StaticFieldWithDefault = 200;

    // 静态只读字段
    public static readonly int StaticReadOnlyFieldWithDefault = 300;

    readonly int _instanceFieldNoDefault;

    // 只读实例字段
    public readonly int ReadOnlyFieldWithDefault = 500;
    public bool BoolFieldWithDefault = true;

    public float FloatFieldWithDefault = 3.14f;

    // 其他类型字段
    public string StringFieldWithDefault = "Hello";

    // 构造函数
    public TestClass()
    {
        // 在构造函数中初始化一些字段
        if (_instanceFieldNoDefault == 0)
        {
            _instanceFieldNoDefault = 999;
        }
    }
}
```

---

> 在无参构造函数中进行初始化的字段也被算入自定义默认值

``` markdown
| 字段完整签名 |
| :---------- |
| `public const int CONST_FIELD = 100;` |
| `public static int StaticFieldWithDefault = 200;` |
| `public static readonly int StaticReadOnlyFieldWithDefault = 300;` |
| `private readonly int _instanceFieldNoDefault = 999;` |
| `public readonly int ReadOnlyFieldWithDefault = 500;` |
| `public bool BoolFieldWithDefault = true;` |
| `public float FloatFieldWithDefault = 3.14f;` |
| `public string StringFieldWithDefault = "Hello";` |
