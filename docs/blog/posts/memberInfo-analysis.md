---
date: 2025-09-24
title: "Member Info Analysis"
description: "分析成员信息，不断完善的文章"
categories:
  - 永久
Tags:
  - 反射
author:
  - Yuumix
---

# Member Info Analysis

分析 `MemberInfo` 包含的成员，查看可以通过 `MemberInfo` 获取的信息。

<!--more-->

## `public virtual IEnumerable<CustomAttributeData> CustomAttributes`

获取 `MemberInfo` 的自定义属性数据。

### `CustomAttributeData`

`CustomAttributeData` 是自定义属性数据，它提供了在运行时检查自定义属性信息的能力，而无需实际加载这些属性。这在 Unity 中特别有用，因为 Unity 的序列化系统和编辑器功能经常需要在不实际实例化属性的情况下检查它们。

#### 使用示例

在Unity开发中，`CustomAttributeData` 有多种实用场景。以下是几个具体的使用示例：

##### 1. 检查字段上的序列化属性

```csharp
using UnityEngine;
using System.Reflection;
using System.Collections.Generic;

public class SerializationChecker : MonoBehaviour
{
    [SerializeField] private int health;
    [HideInInspector] private string secretData;
    private float speed; // 非序列化字段
    
    void Start()
    {
        CheckSerializedFields();
    }
    
    void CheckSerializedFields()
    {
        var fields = GetType().GetFields(BindingFlags.Instance | BindingFlags.Public | BindingFlags.NonPublic);
        
        foreach (var field in fields)
        {
            bool isSerialized = false;
            bool hasHideInInspector = false;
            
            // 检查字段是否有SerializeField或HideInInspector属性
            foreach (var attrData in field.CustomAttributes)
            {
                if (attrData.AttributeType == typeof(SerializeField))
                {
                    isSerialized = true;
                }
                else if (attrData.AttributeType == typeof(HideInInspector))
                {
                    hasHideInInspector = true;
                }
            }
            
            // 公共字段默认是序列化的，除非有HideInInspector
            if (field.IsPublic && !hasHideInInspector)
            {
                isSerialized = true;
            }
            
            Debug.Log($"字段 {field.Name}: Serialized = {isSerialized}, HideInInspector = {hasHideInInspector}");
        }
    }
}
```

##### 2. 自定义 Inspector 中读取自定义属性

```csharp
using UnityEngine;
using System;
using System.Reflection;

// 自定义属性
[AttributeUsage(AttributeTargets.Field)]
public class ProgressBarAttribute : PropertyAttribute
{
    public readonly float minValue;
    public readonly float maxValue;
    public readonly string label;
    
    public ProgressBarAttribute(float min, float max, string labelText = "")
    {
        minValue = min;
        maxValue = max;
        label = labelText;
    }
}

// 使用示例
public class PlayerStats : MonoBehaviour
{
    [ProgressBar(0, 100, "Health")]
    public float health = 50;
    
    [ProgressBar(0, 100, "Mana")]
    public float mana = 80;
    
    [ProgressBar(0, 10, "Level")]
    public int level = 1;
}

// 在Editor中读取属性数据
#if UNITY_EDITOR
using UnityEditor;

[CustomPropertyDrawer(typeof(ProgressBarAttribute))]
public class ProgressBarDrawer : PropertyDrawer
{
    public override void OnGUI(Rect position, SerializedProperty property, GUIContent label)
    {
        // 获取ProgressBarAttribute的实例数据
        var attributeData = GetCustomAttributeData(property);
        if (attributeData != null)
        {
            // 读取构造函数参数
            var args = attributeData.ConstructorArguments;
            float min = (float)args[0].Value;
            float max = (float)args[1].Value;
            string customLabel = args.Count > 2 ? (string)args[2].Value : "";
            
            // 使用参数绘制进度条
            EditorGUI.LabelField(new Rect(position.x, position.y, position.width, 16), 
                string.IsNullOrEmpty(customLabel) ? label.text : customLabel);
            
            if (property.propertyType == SerializedPropertyType.Float)
            {
                EditorGUI.ProgressBar(new Rect(position.x, position.y + 16, position.width, 16), 
                    Mathf.InverseLerp(min, max, property.floatValue), 
                    property.floatValue.ToString("F2"));
            }
            else if (property.propertyType == SerializedPropertyType.Integer)
            {
                EditorGUI.ProgressBar(new Rect(position.x, position.y + 16, position.width, 16), 
                    Mathf.InverseLerp(min, max, property.intValue), 
                    property.intValue.ToString());
            }
        }
        else
        {
            EditorGUI.PropertyField(position, property, label);
        }
    }
    
    public override float GetPropertyHeight(SerializedProperty property, GUIContent label)
    {
        return 32; // 高度为标准高度的两倍
    }
    
    private CustomAttributeData GetCustomAttributeData(SerializedProperty property)
    {
        // 通过反射获取字段的CustomAttributeData
        var targetObject = property.serializedObject.targetObject;
        var fieldInfo = targetObject.GetType().GetField(property.name, 
            BindingFlags.Instance | BindingFlags.Public | BindingFlags.NonPublic);
        
        if (fieldInfo != null)
        {
            foreach (var attrData in fieldInfo.CustomAttributes)
            {
                if (attrData.AttributeType == typeof(ProgressBarAttribute))
                {
                    return attrData;
                }
            }
        }
        return null;
    }
}
#endif
```

##### 3. 运行时验证属性配置

```csharp
using UnityEngine;
using System;
using System.Reflection;
using System.Collections.Generic;

// 验证属性
[AttributeUsage(AttributeTargets.Field)]
public class ValidateAttribute : PropertyAttribute
{
    public readonly float minValue;
    public readonly float maxValue;
    
    public ValidateAttribute(float min, float max)
    {
        minValue = min;
        maxValue = max;
    }
}

// 使用示例
public class GameSettings : MonoBehaviour
{
    [Validate(0, 100)]
    public float volume = 50;
    
    [Validate(1, 10)]
    public int difficulty = 5;
    
    [Validate(0, 1000)]
    public int score = 0;
    
    void OnValidate()
    {
        ValidateFields();
    }
    
    void ValidateFields()
    {
        var fields = GetType().GetFields(BindingFlags.Instance | BindingFlags.Public);
        
        foreach (var field in fields)
        {
            foreach (var attrData in field.CustomAttributes)
            {
                if (attrData.AttributeType == typeof(ValidateAttribute))
                {
                    // 获取验证参数
                    var args = attrData.ConstructorArguments;
                    float min = (float)args[0].Value;
                    float max = (float)args[1].Value;
                    
                    // 获取字段值并验证
                    var value = field.GetValue(this);
                    if (value is float fValue)
                    {
                        if (fValue < min || fValue > max)
                        {
                            Debug.LogWarning($"字段 {field.Name} 的值 {fValue} 超出范围 [{min}, {max}]");
                            field.SetValue(this, Mathf.Clamp(fValue, min, max));
                        }
                    }
                    else if (value is int iValue)
                    {
                        if (iValue < min || iValue > max)
                        {
                            Debug.LogWarning($"字段 {field.Name} 的值 {iValue} 超出范围 [{min}, {max}]");
                            field.SetValue(this, (int)Mathf.Clamp(iValue, min, max));
                        }
                    }
                }
            }
        }
    }
}
```

##### 4. 基于属性的条件编译

```csharp
using UnityEngine;
using System;
using System.Reflection;

// 条件编译属性
[AttributeUsage(AttributeTargets.Method)]
public class ConditionalAttribute : System.Attribute
{
    public readonly string condition;
    
    public ConditionalAttribute(string conditionName)
    {
        condition = conditionName;
    }
}

// 平台特定属性
[AttributeUsage(AttributeTargets.Method)]
public class PlatformSpecificAttribute : System.Attribute
{
    public readonly RuntimePlatform platform;
    
    public PlatformSpecificAttribute(RuntimePlatform targetPlatform)
    {
        platform = targetPlatform;
    }
}

public class PlatformManager : MonoBehaviour
{
    [Conditional("DEBUG_MODE")]
    public void DebugLog(string message)
    {
        Debug.Log($"[Debug] {message}");
    }
    
    [PlatformSpecific(RuntimePlatform.WindowsPlayer)]
    public void WindowsOnlyFunction()
    {
        Debug.Log("This function only runs on Windows");
    }
    
    [PlatformSpecific(RuntimePlatform.Android)]
    public void AndroidOnlyFunction()
    {
        Debug.Log("This function only runs on Android");
    }
    
    void Start()
    {
        // 检查哪些方法可以在此平台上运行
        CheckAvailableMethods();
    }
    
    void CheckAvailableMethods()
    {
        var methods = GetType().GetMethods(BindingFlags.Instance | BindingFlags.Public);
        
        foreach (var method in methods)
        {
            // 检查平台特定属性
            foreach (var attrData in method.CustomAttributes)
            {
                if (attrData.AttributeType == typeof(PlatformSpecificAttribute))
                {
                    var platformArg = attrData.ConstructorArguments[0].Value;
                    RuntimePlatform targetPlatform = (RuntimePlatform)platformArg;
                    
                    if (Application.platform == targetPlatform)
                    {
                        Debug.Log($"方法 {method.Name} 可以在此平台 ({Application.platform}) 上运行");
                    }
                    else
                    {
                        Debug.Log($"方法 {method.Name} 仅支持 {targetPlatform} 平台");
                    }
                }
                else if (attrData.AttributeType == typeof(ConditionalAttribute))
                {
                    var conditionArg = attrData.ConstructorArguments[0].Value as string;
                    
                    // 检查条件是否满足（简化示例）
                    bool conditionMet = conditionArg == "DEBUG_MODE" && Debug.isDebugBuild;
                    
                    Debug.Log($"方法 {method.Name} 的条件 '{conditionArg}' 满足: {conditionMet}");
                }
            }
        }
    }
}
```

##### 5. 自动化文档生成

```csharp
using UnityEngine;
using System;
using System.Reflection;
using System.Text;

// 文档属性
[AttributeUsage(AttributeTargets.All)]
public class DocumentAttribute : Attribute
{
    public readonly string description;
    public readonly string category;
    public readonly string version;
    
    public DocumentAttribute(string desc, string cat = "General", string ver = "1.0")
    {
        description = desc;
        category = cat;
        version = ver;
    }
}

// 使用示例
[Document("玩家控制器类", "Gameplay", "1.2")]
public class PlayerController : MonoBehaviour
{
    [Document("玩家的生命值", "Stats", "1.0")]
    public int health = 100;
    
    [Document("玩家的移动速度", "Movement", "1.1")]
    public float speed = 5.0f;
    
    [Document("玩家跳跃功能", "Movement", "1.0")]
    public void Jump()
    {
        // 跳跃逻辑
    }
    
    [Document("玩家攻击功能", "Combat", "1.2")]
    public void Attack()
    {
        // 攻击逻辑
    }
    
    // 生成文档的方法
    public string GenerateDocumentation()
    {
        StringBuilder doc = new StringBuilder();
        
        // 类文档
        doc.AppendLine($"# {GetType().Name}");
        foreach (var attrData in GetType().CustomAttributes)
        {
            if (attrData.AttributeType == typeof(DocumentAttribute))
            {
                var desc = attrData.ConstructorArguments[0].Value as string;
                var cat = attrData.ConstructorArguments.Count > 1 ? attrData.ConstructorArguments[1].Value as string : "General";
                var ver = attrData.ConstructorArguments.Count > 2 ? attrData.ConstructorArguments[2].Value as string : "1.0";
                
                doc.AppendLine($"描述: {desc}");
                doc.AppendLine($"分类: {cat}");
                doc.AppendLine($"版本: {ver}");
                break;
            }
        }
        
        doc.AppendLine("\n## 字段");
        // 字段文档
        var fields = GetType().GetFields(BindingFlags.Instance | BindingFlags.Public);
        foreach (var field in fields)
        {
            foreach (var attrData in field.CustomAttributes)
            {
                if (attrData.AttributeType == typeof(DocumentAttribute))
                {
                    var desc = attrData.ConstructorArguments[0].Value as string;
                    var cat = attrData.ConstructorArguments.Count > 1 ? attrData.ConstructorArguments[1].Value as string : "General";
                    var ver = attrData.ConstructorArguments.Count > 2 ? attrData.ConstructorArguments[2].Value as string : "1.0";
                    
                    doc.AppendLine($"### {field.Name}");
                    doc.AppendLine($"类型: {field.FieldType.Name}");
                    doc.AppendLine($"描述: {desc}");
                    doc.AppendLine($"分类: {cat}");
                    doc.AppendLine($"版本: {ver}\n");
                    break;
                }
            }
        }
        
        doc.AppendLine("## 方法");
        // 方法文档
        var methods = GetType().GetMethods(BindingFlags.Instance | BindingFlags.Public);
        foreach (var method in methods)
        {
            // 跳过Unity内置方法
            if (method.DeclaringType == typeof(MonoBehaviour) || method.DeclaringType == typeof(Component) || 
                method.DeclaringType == typeof(Object) || method.DeclaringType == typeof(object))
                continue;
                
            foreach (var attrData in method.CustomAttributes)
            {
                if (attrData.AttributeType == typeof(DocumentAttribute))
                {
                    var desc = attrData.ConstructorArguments[0].Value as string;
                    var cat = attrData.ConstructorArguments.Count > 1 ? attrData.ConstructorArguments[1].Value as string : "General";
                    var ver = attrData.ConstructorArguments.Count > 2 ? attrData.ConstructorArguments[2].Value as string : "1.0";
                    
                    doc.AppendLine($"### {method.Name}");
                    doc.AppendLine($"返回类型: {method.ReturnType.Name}");
                    doc.AppendLine($"描述: {desc}");
                    doc.AppendLine($"分类: {cat}");
                    doc.AppendLine($"版本: {ver}\n");
                    break;
                }
            }
        }
        
        return doc.ToString();
    }
}
```

这些示例展示了在Unity中使用`CustomAttributeData`的多种方式：

1. **序列化检查**：检查字段是否会被Unity序列化
2. **自定义Inspector**：在Editor中读取属性数据来创建自定义绘制逻辑
3. **运行时验证**：在运行时验证字段值是否符合属性定义的约束
4. **条件编译**：基于属性决定方法是否应该执行
5. **文档生成**：使用属性来生成自动化文档

通过这些示例，您可以看到`CustomAttributeData`在Unity开发中的强大功能，它允许我们在不实例化属性的情况下获取属性信息，这对于创建灵活和可维护的代码非常有用。
