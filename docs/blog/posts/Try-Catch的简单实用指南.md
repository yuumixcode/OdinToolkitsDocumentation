---
authors: [Yuumix]
date: 2025-09-27
categories:
  - Unity
  - csharp
tags:
  - Try-Catch
slug: try-catch-practice-guide
---
# Try-Catch 实践指南：在 Unity C# 中优雅处理错误

## 引言

在 Unity 开发中，错误处理至关重要。一个健壮的游戏应该能够优雅地处理意外情况，而不是直接崩溃或产生难以调试的行为。Try-Catch 作为 C# 中的核心异常处理机制，配合 Odin Inspector 插件，可以帮助我们构建更稳定、更易于调试的 Unity 项目。

<!-- more -->

## 一、Try-Catch 基础用法

### 基本语法

```csharp
try
{
    // 可能发生错误的代码
    riskyOperation();
}
catch (SpecificException ex)
{
    // 处理特定类型的异常
    Debug.LogError($"发生特定错误: {ex.Message}");
}
catch (Exception ex)
{
    // 处理其他所有异常
    Debug.LogError($"发生错误: {ex.Message}");
}
finally
{
    // 无论是否发生错误都会执行的代码
    cleanupResources();
}
```

### 在 Unity 中的基本实践步骤

1. **识别风险代码块** - 确定哪些操作可能失败：
   - 资源加载（`Resources.Load`、`AssetBundle.Load`）
   - 文件操作（`File.ReadAllText`）
   - 网络请求
   - 反射操作
   - 解析数据（JSON、XML）
2. **捕获具体异常** - 始终优先捕获具体异常类型：

```csharp
try
{
    var prefab = Resources.Load<GameObject>("MyPrefab");
    Instantiate(prefab);
}
catch (ResourceLoadException ex)
{
    Debug.LogError($"资源加载失败: {ex.Message}");
}
catch (ArgumentNullException ex)
{
    Debug.LogError($"实例化失败，预制体为空: {ex.Message}");
}
```

1. **适当处理或传递异常** - 根据情况决定是在当前位置处理异常还是向上传递：

```csharp
// 向上传递异常让调用者处理
public GameObject LoadAndInstantiate(string resourcePath)
{
    try
    {
        var prefab = Resources.Load<GameObject>(resourcePath);
        if (prefab == null)
            throw new Exception($"未找到资源: {resourcePath}");
            
        return Instantiate(prefab);
    }
    catch (Exception ex)
    {
        // 添加额外上下文信息后重新抛出
        throw new Exception($"加载并实例化资源失败: {resourcePath}", ex);
    }
}
```

## 二、Unity 中的常见应用场景

### 1. 资源加载

```csharp
public T LoadResource<T>(string path) where T : UnityEngine.Object
{
    try
    {
        T resource = Resources.Load<T>(path);
        if (resource == null)
        {
            throw new System.Exception($"资源加载失败: {path}，类型: {typeof(T).Name}");
        }
        return resource;
    }
    catch (System.Exception ex)
    {
        Debug.LogError($"[ResourceLoader] 加载资源时出错: {ex.Message}");
        return null;
    }
}
```

### 2. 协程中的错误处理

```csharp
IEnumerator LoadDataCoroutine(string url)
{
    UnityWebRequest request = UnityWebRequest.Get(url);
    
    try
    {
        yield return request.SendWebRequest();

        if (request.result != UnityWebRequest.Result.Success)
        {
            throw new System.Exception($"网络请求失败: {request.error}");
        }
        
        ProcessData(request.downloadHandler.text);
    }
    catch (System.Exception ex)
    {
        Debug.LogError($"[DataLoader] 加载数据失败: {ex.Message}");
        ShowErrorMessageToPlayer("数据加载失败，请稍后重试");
    }
    finally
    {
        request.Dispose();
    }
}
```

### 3. 数据解析

```csharp
public T ParseJson<T>(string jsonString)
{
    try
    {
        return JsonUtility.FromJson<T>(jsonString);
    }
    catch (System.ArgumentException ex)
    {
        Debug.LogError($"[JsonParser] JSON 解析失败: {ex.Message}");
        Debug.LogError($"无效的 JSON: {jsonString}");
        return default(T);
    }
}
```

## 三、结合 Odin Inspector 增强错误处理

Odin Inspector 可以帮助我们在编辑器中更好地处理和展示错误信息，提升开发效率。

### 1. 在 Inspector 中显示错误信息

```csharp
using Sirenix.OdinInspector;
using UnityEngine;

public class ErrorHandlingExample : MonoBehaviour
{
    [SerializeField] private string resourcePath;
    
    [ShowInInspector, ReadOnly] private string lastError;
    
    [Button("加载资源")]
    public void LoadResource()
    {
        try
        {
            lastError = string.Empty; // 清除之前的错误
            var resource = Resources.Load<GameObject>(resourcePath);
            
            if (resource == null)
                throw new System.Exception("资源为空或不存在");
                
            Instantiate(resource, transform);
        }
        catch (System.Exception ex)
        {
            lastError = ex.Message; // 在 Inspector 中显示错误
            Debug.LogError($"加载失败: {ex.Message}");
        }
    }
}
```

### 2. 条件验证与错误提示

```csharp
using Sirenix.OdinInspector;
using UnityEngine;

public class DataProcessor : MonoBehaviour
{
    [SerializeField, Required("数据文件路径不能为空!")]
    private string dataFilePath;
    
    [Button("处理数据")]
    [GUIColor(0.8f, 0.2f, 0.2f)]
    public void ProcessData()
    {
        try
        {
            // 使用 Odin 的验证确保路径有效
            if (string.IsNullOrEmpty(dataFilePath))
                throw new System.Exception("数据文件路径不能为空");
                
            string data = System.IO.File.ReadAllText(dataFilePath);
            // 处理数据...
            
            Debug.Log("数据处理成功");
        }
        catch (System.Exception ex)
        {
            // 在编辑器中显示错误弹窗
            Sirenix.Utilities.Editor.EditorUtility.ShowDialog(
                "处理失败", 
                ex.Message, 
                "确定");
        }
    }
}
```

### 3. 错误日志与重试机制

```csharp
using Sirenix.OdinInspector;
using System;
using UnityEngine;

public class NetworkManager : MonoBehaviour
{
    [SerializeField] private string apiUrl;
    
    [ShowInInspector, FoldoutGroup("错误日志")]
    private string lastErrorLog;
    
    [Button("发送请求")]
    public void SendRequest()
    {
        StartCoroutine(TrySendRequest());
    }
    
    private System.Collections.IEnumerator TrySendRequest()
    {
        int retryCount = 0;
        const int maxRetries = 3;
        
        while (retryCount < maxRetries)
        {
            try
            {
                using (var request = UnityWebRequest.Get(apiUrl))
                {
                    yield return request.SendWebRequest();
                    
                    if (request.result != UnityWebRequest.Result.Success)
                    {
                        throw new Exception($"请求失败: {request.error}");
                    }
                    
                    Debug.Log("请求成功: " + request.downloadHandler.text);
                    lastErrorLog = "请求成功";
                    yield break;
                }
            }
            catch (Exception ex)
            {
                retryCount++;
                lastErrorLog = $"尝试 {retryCount}/{maxRetries} 失败: {ex.Message}";
                Debug.LogWarning(lastErrorLog);
                
                if (retryCount >= maxRetries)
                {
                    lastErrorLog = $"最终失败: {ex.Message}";
                    ShowErrorPopup(lastErrorLog);
                }
                else
                {
                    // 等待后重试
                    yield return new WaitForSeconds(1f);
                }
            }
        }
    }
    
    [Button("显示错误弹窗"), DisableIf("IsLastErrorEmpty")]
    private void ShowErrorPopup(string message)
    {
        Sirenix.Utilities.Editor.EditorUtility.ShowDialog(
            "网络错误", 
            message, 
            "确定");
    }
    
    private bool IsLastErrorEmpty()
    {
        return string.IsNullOrEmpty(lastErrorLog) || lastErrorLog.Contains("成功");
    }
}
```

## 四、Try-Catch 的强大之处

### 1. 防止游戏崩溃，提升用户体验

Unity 游戏中未处理的异常可能导致：

- 编辑器崩溃
- 游戏在某些设备上闪退
- 玩家进度丢失

Try-Catch 可以捕获这些异常，让游戏有机会优雅地恢复或提示用户。

### 2. 精确定位问题，加速调试

通过捕获异常并记录详细信息（包括堆栈跟踪），可以快速定位问题：

```csharp
try
{
    // 可能出错的代码
}
catch (Exception ex)
{
    Debug.LogError($"错误: {ex.Message}\n堆栈跟踪: {ex.StackTrace}");
    // 可以将错误信息发送到服务器进行分析
}
```

### 3. 分离正常逻辑与错误处理

没有 Try-Catch 时，代码可能充斥着各种检查：

```csharp
// 没有 Try-Catch 的代码
var prefab = Resources.Load<GameObject>(path);
if (prefab == null)
{
    Debug.LogError("资源为空");
    return;
}

var instance = Instantiate(prefab);
if (instance == null)
{
    Debug.LogError("实例化失败");
    return;
}

var component = instance.GetComponent<MyComponent>();
if (component == null)
{
    Debug.LogError("组件缺失");
    Destroy(instance);
    return;
}
```

使用 Try-Catch 后，代码更清晰：

```csharp
// 使用 Try-Catch 的代码
try
{
    var prefab = Resources.Load<GameObject>(path) ?? 
        throw new Exception("资源为空");
        
    var instance = Instantiate(prefab) ?? 
        throw new Exception("实例化失败");
        
    var component = instance.GetComponent<MyComponent>() ?? 
        throw new Exception("组件缺失");
        
    // 正常逻辑...
}
catch (Exception ex)
{
    Debug.LogError($"操作失败: {ex.Message}");
    // 清理工作
}
```

### 4. 实现复杂的错误恢复策略

Try-Catch 使实现重试、回退等复杂错误处理策略变得简单：

```csharp
// 带重试机制的资源加载
public T LoadWithRetry<T>(string path, int maxRetries = 3) where T : UnityEngine.Object
{
    int attempts = 0;
    
    while (attempts < maxRetries)
    {
        try
        {
            attempts++;
            var resource = Resources.Load<T>(path);
            
            if (resource == null)
                throw new Exception("资源加载返回空");
                
            return resource;
        }
        catch (Exception ex)
        {
            Debug.LogWarning($"尝试 {attempts} 加载失败: {ex.Message}");
            
            if (attempts >= maxRetries)
            {
                Debug.LogError("达到最大重试次数，加载失败");
                return null;
            }
            
            // 等待一小段时间再重试
            System.Threading.Thread.Sleep(100);
        }
    }
    
    return null;
}
```

## 五、最佳实践与注意事项

1. **不要过度使用 Try-Catch** - 只在可能发生不可预测错误的地方使用，不要用它替代正常的条件检查。
2. **避免捕获所有异常** - 不要使用空的 `catch {}` 或捕获 `Exception` 而不做处理，这会隐藏真正的问题。
3. **注意性能影响** - Try-Catch 会带来轻微的性能开销，避免在每帧执行的代码（如 `Update`）中过度使用。
4. **清理资源** - 始终在 `finally` 块中释放资源（如文件流、网络请求）。
5. **提供有用的错误信息** - 错误信息应包含足够的上下文，帮助调试。
6. **区分开发和生产环境** - 在开发环境中可以抛出详细错误，在生产环境中则应显示友好提示并记录错误。

```csharp
try
{
    // 风险操作
}
catch (Exception ex)
{
    // 开发环境 - 显示详细错误
#if UNITY_EDITOR
    Debug.LogError($"详细错误: {ex}");
    Sirenix.Utilities.Editor.EditorUtility.ShowDialog("错误", ex.Message, "确定");
#else
    // 生产环境 - 显示友好信息
    Debug.LogError($"错误: {ex.Message}");
    ShowPlayerFriendlyError("发生错误，请重试或联系支持");
#endif
}
```

## 结语

Try-Catch 是 Unity 开发中处理错误的强大工具，当与 Odin Inspector 结合使用时，不仅能让你的游戏更健壮，还能显著提升开发效率和调试体验。通过合理使用异常处理，你可以构建出更稳定、更专业的 Unity 游戏。

记住，良好的错误处理不是事后弥补，而是应该在开发初期就纳入设计考量的重要部分。
