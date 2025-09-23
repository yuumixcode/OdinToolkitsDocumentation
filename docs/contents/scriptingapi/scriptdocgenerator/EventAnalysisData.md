# `EventAnalysisData`

## 介绍

- 种类: `class`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.Modules`

``` csharp
[Serializable]
public class EventAnalysisData : Yuumix.OdinToolkits.Modules.MemberAnalysisData
```

## 构造方法

| 构造方法签名 [仅包含公共实例方法] | 注释 |
| :--- | :--- |
| `public EventAnalysisData()` | 无 |

## 非构造方法

### 声明的普通方法

| 普通方法名称 | 注释 |
| :--- | :--- | 
| `public static EventAnalysisData FromEventInfo` | 无 |

### 继承的普通方法

| 普通方法名称 | 注释 | 声明方法的类 |
| :--- | :--- | :--- |
| `public virtual bool Equals` | 无 | `System.Object` |
| `public virtual int GetHashCode` | 无 | `System.Object` |
| `public Type GetType` | 无 | `System.Object` |
| `public bool IsAPI` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |
| `public bool IsFromInheritMember` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |
| `public virtual string ToString` | 无 | `System.Object` |
| `protected virtual void Finalize` | 无 | `System.Object` |
| `protected Object MemberwiseClone` | 无 | `System.Object` |

### 所有方法签名总览

| 方法签名 |
| :--- | 
| `public static EventAnalysisData FromEventInfo(EventInfo eventInfo, Type type);` |
| `public virtual bool Equals(Object obj);` |
| `public virtual int GetHashCode();` |
| `public Type GetType();` |
| `public bool IsAPI();` |
| `public bool IsFromInheritMember();` |
| `public virtual string ToString();` |
| `protected virtual void Finalize();` |
| `protected Object MemberwiseClone();` |

## 属性

### 继承的属性

| 属性名称 | 注释 | 声明属性的类 | 
| :--- | :--- | :--- |
| `public string AccessModifier` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |

### 所有属性签名总览

| 属性签名 |
| :--- | 
| `public string AccessModifier { public get; }` |
## 字段

### 声明的普通字段

| 字段名称 | 注释 | 
| :--- | :--- | 
| `public bool isStatic;` | 无 |

### 继承的普通字段

| 字段名称 | 注释 | 声明字段的类 |
| :--- | :--- | :--- |
| `public string belongToType;` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |
| `public string chineseSummary;` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |
| `public string declaringType;` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |
| `public string englishSummary;` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |
| `public string fullDeclaration;` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |
| `public string fullSignature;` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |
| `public bool isObsolete;` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |
| `public AccessModifierType memberAccessModifierType;` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |
| `public MemberTypes memberType;` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |
| `public string name;` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |
| `public string partSignature;` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |
| `public string returnType;` | 无 | `Yuumix.OdinToolkits.Modules.MemberAnalysisData` |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
