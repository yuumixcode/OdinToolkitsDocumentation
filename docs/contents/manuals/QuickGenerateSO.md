# QuickGenerateSO

## 用法

  1. 选择继承了 `ScriptableObject` 的脚本
  2. 右键选择 `Create SO Asset From Selected`
  3. 在当前目录生成一个 SO 资源

!!! tip "提示"

    1. 支持单选和多选。
         1. 单选 SO 脚本生成时可以立即设置文件名。
         2. 多选 SO 脚本生成时将直接生成资源文件，使用默认命名。
    2. 只有当选择的资源中包含有继承 `ScriptableObject` 的脚本文件时才可以点击 `Create SO Asset From Selected`。

!!! warning "限制"

    内部使用 `MonoScript.GetClass()` 方法获取 `Type` 值，此方法返回“主类”的 `Type` 值，要求是 `public`，且和文件名相同的类，因此使用时最好一个脚本文件中只包含一个继承 `ScriptableObject` 的类。

## 流程图

### 验证是否可以点击执行按钮操作

``` mermaid
flowchart TD
  A(Start) --> B{选择的资源是否为空？};
  B -->|是| C[返回 false，无法点击执行按钮操作];
  B -->|否| D[遍历选择的所有资源];
  D --> F{这个资源是否为MonoScript类型？};
  F -->|是| E{GetClass（）方法是否可以获得一个具体类型？};
  F -->|否| G[选择下一个资源];
  G --> D;
  E -->|否| G;
  H -->|否| G;
  E -->|是| H{获得的这个类是否继承 ScriptableObject?};
  H -->|是| I(返回 true，可以点击);
  C --> M(End);
  I --> M;
```

### 生成 SO 资源

```mermaid
flowchart TD
  A(Start) --> B{选择资源的数量是否为1个}
  B --> |是| C[生成一个SO资源，且立即重命名编辑]
  B --> |否| D[遍历所有资源]
  D --> F[生成SO资源，使用默认命名，并刷新]
  C --> G(End)
  F --> G
```
