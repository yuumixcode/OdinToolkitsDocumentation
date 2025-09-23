# `TypeAnalyzerUtility`

## 介绍

- 种类: `static class`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.Modules`

``` csharp
public static class TypeAnalyzerUtility : System.Object
```

## 非构造方法

### 声明的普通方法

| 普通方法名称 | 注释 |
| :--- | :--- | 
| `public static AccessModifierType GetEventAccessModifierType` | 无 |
| `public static AccessModifierType GetFieldAccessModifierType` | 无 |
| `public static string GetFullMethodName` | 无 |
| `public static string GetParamsNamesWithDefaultValue` | 无 |
| `public static AccessModifierType GetPropertyAccessModifierType` | 无 |
| `public static string GetReadableEventReturnType` | 无 |
| `public static bool IsConstantField` | 无 |
| `public static bool IsFromInterfaceMethod` | 无 |
| `public static bool IsInheritedOverrideFromAncestor` | 无 |
| `public static void IsStaticEvent` | 无 |

### 继承的普通方法

| 普通方法名称 | 注释 | 声明方法的类 |
| :--- | :--- | :--- |
| `public virtual bool Equals` | 无 | `System.Object` |
| `public virtual int GetHashCode` | 无 | `System.Object` |
| `public Type GetType` | 无 | `System.Object` |
| `public virtual string ToString` | 无 | `System.Object` |
| `protected virtual void Finalize` | 无 | `System.Object` |
| `protected Object MemberwiseClone` | 无 | `System.Object` |

### 所有方法签名总览

| 方法签名 |
| :--- | 
| `public static AccessModifierType GetEventAccessModifierType(EventInfo eventInfo);` |
| `public static AccessModifierType GetFieldAccessModifierType(FieldInfo field);` |
| `public static string GetFullMethodName(MethodBase method, string extensionMethodPrefix);` |
| `public static string GetParamsNamesWithDefaultValue(MethodBase method);` |
| `public static AccessModifierType GetPropertyAccessModifierType(PropertyInfo property);` |
| `public static string GetReadableEventReturnType(EventInfo eventInfo);` |
| `public static bool IsConstantField(FieldInfo field);` |
| `public static bool IsFromInterfaceMethod(MethodInfo method);` |
| `public static bool IsInheritedOverrideFromAncestor(MethodInfo method, Type currentType);` |
| `public static void IsStaticEvent(EventInfo eventInfo, EventAnalysisData eventData);` |
| `public virtual bool Equals(Object obj);` |
| `public virtual int GetHashCode();` |
| `public Type GetType();` |
| `public virtual string ToString();` |
| `protected virtual void Finalize();` |
| `protected Object MemberwiseClone();` |

## 字段

### 声明的普通字段

| 字段名称 | 注释 | 
| :--- | :--- | 
| `public readonly Dictionary<Type, string> TypeAliasMap;` | 无 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
