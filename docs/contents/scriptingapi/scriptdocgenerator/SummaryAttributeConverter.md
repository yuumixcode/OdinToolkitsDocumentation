# `SummaryAttributeConverter`

## 介绍

- 种类: `static class`
- 所在程序集: `Yuumix.OdinToolkits.Editor`
- 所在命名空间: `Yuumix.OdinToolkits.Modules.Editor`

``` csharp
public static class SummaryAttributeConverter : System.Object
```

## 非构造方法

### 声明的普通方法

| 普通方法名称 | 注释 |
| :--- | :--- | 
| `public static string GetCodeWithRemoveAllChineseSummary` | 无 |
| `public static string GetCodeWithSyncToChineseSummary` | 无 |
| `public static ValueTuple<string, string> TestSplitCodeParts` | 无 |
| `public static void TestXmlToDeclarationEndRegex` | 无 |
| `public static void WriteRemoveChineseSummaryText` | 无 |
| `public static void WriteSyncChineseSummaryText` | 无 |

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
| `public static string GetCodeWithRemoveAllChineseSummary(string sourceCode);` |
| `public static string GetCodeWithSyncToChineseSummary(string sourceCode);` |
| `public static ValueTuple<string, string> TestSplitCodeParts(string otherXmlAndAttributesAndDeclaration, string newLine);` |
| `public static void TestXmlToDeclarationEndRegex(string sourceCode);` |
| `public static void WriteRemoveChineseSummaryText(string filePath);` |
| `public static void WriteSyncChineseSummaryText(string filePath);` |
| `public virtual bool Equals(Object obj);` |
| `public virtual int GetHashCode();` |
| `public Type GetType();` |
| `public virtual string ToString();` |
| `protected virtual void Finalize();` |
| `protected Object MemberwiseClone();` |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
