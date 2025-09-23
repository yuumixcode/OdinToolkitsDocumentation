# `ScriptDocGenInspectorSO`

## 介绍

- 种类: `class`
- 所在程序集: `Yuumix.OdinToolkits.Editor`
- 所在命名空间: `Yuumix.OdinToolkits.Modules.Editor`

``` csharp
public class ScriptDocGenInspectorSO : Yuumix.OdinToolkits.Core.Editor.OdinEditorScriptableSingleton<ScriptDocGenInspectorSO>, Yuumix.OdinToolkits.Core.IOdinToolkitsEditorReset, UnityEngine.ISerializationCallbackReceiver
```

## 构造方法

| 构造方法签名 [仅包含公共实例方法] | 注释 |
| :--- | :--- |
| `public ScriptDocGenInspectorSO()` | 无 |

## 非构造方法

### 声明的普通方法

| 普通方法名称 | 注释 |
| :--- | :--- | 
| `public void AnalyzeButton` | 无 |
| `public void EditorReset` | 无 |
| `public void GenerateButton` | 无 |

### 继承的普通方法

| 普通方法名称 | 注释 | 声明方法的类 |
| :--- | :--- | :--- |
| `public override bool Equals` | 无 | `UnityEngine.Object` |
| `public override int GetHashCode` | 无 | `UnityEngine.Object` |
| `public int GetInstanceID` | 无 | `UnityEngine.Object` |
| `public Type GetType` | 无 | `System.Object` |
| `public void SetDirty` | 无 | `UnityEngine.ScriptableObject` |
| `public override string ToString` | 无 | `UnityEngine.Object` |
| `protected virtual void Finalize` | 无 | `System.Object` |
| `protected Object MemberwiseClone` | 无 | `System.Object` |
| `protected virtual void OnAfterDeserialize` | 无 | `Sirenix.OdinInspector.SerializedScriptableObject` |
| `protected virtual void OnBeforeSerialize` | 无 | `Sirenix.OdinInspector.SerializedScriptableObject` |

### 所有方法签名总览

| 方法签名 |
| :--- | 
| `public void AnalyzeButton();` |
| `public void EditorReset();` |
| `public void GenerateButton();` |
| `public override bool Equals(Object other);` |
| `public override int GetHashCode();` |
| `public int GetInstanceID();` |
| `public Type GetType();` |
| `public void SetDirty();` |
| `public override string ToString();` |
| `protected virtual void Finalize();` |
| `protected Object MemberwiseClone();` |
| `protected virtual void OnAfterDeserialize();` |
| `protected virtual void OnBeforeSerialize();` |

## 属性

### 声明的属性

| 属性名称 | 注释 |
| :--- | :--- |

### 继承的属性

| 属性名称 | 注释 | 声明属性的类 | 
| :--- | :--- | :--- |
| `public HideFlags hideFlags` | 无 | `UnityEngine.Object` |
| `public string name` | 无 | `UnityEngine.Object` |

### 所有属性签名总览

| 属性签名 |
| :--- | 
| `public HideFlags hideFlags { public get; public set; }` |
| `public string name { public get; public set; }` |
## 字段

### 常量字段

| 字段完整签名 | 注释 |
| :--- | :--- |
| `public const string DEFAULT_DOC_FOLDER_PATH = Assets/OdinToolkitsData/Editor/Documents/;` | 无 |
| `public const string IDENTIFIER_CN = ## 额外说明;` | 无 |
| `public const string IDENTIFIER_EN = ## Additional Description;` | 无 |

### 声明的普通字段

| 字段名称 | 注释 | 
| :--- | :--- | 
| `public static BilingualData MenuName;` | 无 |
| `public static StringBuilder UserIdentifierParagraph;` | 无 |
| `public Type TargetType;` | 无 |
| `public List<Type> TemporaryTypes;` | 无 |
| `public DocGeneratorSO docGenerator;` | 无 |
| `public string folderPath;` | 无 |
| `public BilingualFooterWidget footerWidget;` | 无 |
| `public BilingualHeaderWidget headerWidget;` | 无 |
| `public string targetAssemblyString;` | 无 |
| `public TypeAnalysisData typeAnalysisData;` | 无 |
| `public List<TypeAnalysisData> typeAnalysisDataList;` | 无 |
| `public ScriptDocGenInspectorSO.TypeSourceEnum typeSource;` | 无 |
| `public TypesConfigSO typesConfig;` | 无 |
| `public string typesConfigSOFolderPath;` | 无 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
