# `TypeCategory`

## 介绍

- 种类: `enum`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.Modules`

``` csharp
public enum TypeCategory : System.Enum, System.IFormattable, System.IComparable, System.IConvertible
```

## 非构造方法

### 继承的普通方法

| 普通方法名称 | 注释 | 声明方法的类 |
| :--- | :--- | :--- |
| `public virtual int CompareTo` | 无 | `System.Enum` |
| `public override bool Equals` | 无 | `System.Enum` |
| `public override int GetHashCode` | 无 | `System.Enum` |
| `public Type GetType` | 无 | `System.Object` |
| `public virtual TypeCode GetTypeCode` | 无 | `System.Enum` |
| `public bool HasFlag` | 无 | `System.Enum` |
| `public override string ToString` | 无 | `System.Enum` |
| `[Overload] public virtual string ToString` | 无 | `System.Enum` |
| `public string ToString` | 无 | `System.Enum` |
| `[Overload] public virtual string ToString` | 无 | `System.Enum` |
| `protected virtual void Finalize` | 无 | `System.Object` |
| `protected Object MemberwiseClone` | 无 | `System.Object` |
| `internal Object GetValue` | 无 | `System.Enum` |

### 所有方法签名总览

| 方法签名 |
| :--- | 
| `public virtual int CompareTo(Object target);` |
| `public override bool Equals(Object obj);` |
| `public override int GetHashCode();` |
| `public Type GetType();` |
| `public virtual TypeCode GetTypeCode();` |
| `public bool HasFlag(Enum flag);` |
| `public override string ToString();` |
| `public virtual string ToString(string format, IFormatProvider provider);` |
| `public string ToString(string format);` |
| `public virtual string ToString(IFormatProvider provider);` |
| `protected virtual void Finalize();` |
| `protected Object MemberwiseClone();` |

## 字段

### 常量字段

| 字段完整签名 | 注释 |
| :--- | :--- |
| `public const TypeCategory Class = 0;` | 无 |
| `public const TypeCategory Delegate = 4;` | 无 |
| `public const TypeCategory Enum = 3;` | 无 |
| `public const TypeCategory Interface = 2;` | 无 |
| `public const TypeCategory Record = 5;` | 无 |
| `public const TypeCategory Struct = 1;` | 无 |
| `public const TypeCategory Unknown = 6;` | 无 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
