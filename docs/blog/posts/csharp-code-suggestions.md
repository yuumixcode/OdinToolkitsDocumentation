---
date: 2025-09-24
description: C# Code Suggestions
categories: 
  - csharp
tags:
  - csharp        
author:
  - Yuumix
slug: csharp-code-suggestions
---

# C# 编码建议

C# 是一种非常灵活且功能丰富的语言，它提供了属性、方法、继承、多态等丰富的特性，使得开发者可以根据具体需求选择不同的方式来构建高效、可维护的代码。本文提供一些编码建议，帮助开发者形成统一的编码风格。

<!--more-->

## 属性与方法的使用时机

在C#开发中，属性和方法都可以用于访问对象的数据，但它们有不同的语义含义和使用场景。正确区分它们的使用时机对于编写高质量的代码至关重要。

### 推荐的编码规范

基于业界最佳实践，建议采用以下规范：

#### 1. 使用属性的情况

##### 1.1 对象状态和特征

```csharp
// 正确：表示对象的基本特征
public class Person 
{
    public string FirstName { get; set; }  // 可读写状态
    public string LastName { get; set; }   // 可读写状态
    public int Age { get; }                // 只读状态（构造函数初始化）
}
```

##### 1.2 简单计算属性

```csharp
// 正确：简单、高性能的计算
public class Rectangle 
{
    public double Width { get; set; }
    public double Height { get; set; }
    public double Area => Width * Height;  // 简单计算属性
}
```

##### 1.3 需要轻量级验证的字段

```csharp
// 正确：简单的验证逻辑
private string _email;
public string Email 
{
    get => _email;
    set => _email = string.IsNullOrEmpty(value) ? 
        throw new ArgumentException("邮箱不能为空") : value;
}
```

#### 2. 使用方法的情况

##### 2.1 复杂业务逻辑

```csharp
// 正确：复杂计算应使用方法
public OrderSummary GenerateOrderSummary(OrderFilter filter)
{
    // 复杂业务逻辑
    return new OrderSummary();
}
```

##### 2.2 有副作用的操作

```csharp
// 正确：会改变系统状态的操作使用方法
public bool SendEmail(Notification notification)
{
    // 发送邮件，有外部副作用
    return true;
}
```

##### 2.3 需要多个参数

```csharp
// 正确：需要多个输入参数时使用方法
public Customer CreateCustomer(string name, string email, string phone, Address address)
{
    return new Customer(name, email, phone, address);
}
```

##### 2.4 可能抛出异常的操作

```csharp
// 正确：可能失败的操作使用方法
public Product FindProduct(string sku)
{
    if (string.IsNullOrEmpty(sku))
        throw new ArgumentException("SKU不能为空");
        
    // 查找逻辑
    return product;
}
```

#### 3. 特殊情况处理

##### 3.1 只读状态

对于只读状态，仍然使用属性而非方法：

```csharp
// 推荐
public DateTime CreatedAt { get; } = DateTime.Now;

// 不推荐
public DateTime GetCreatedAt() => _createdAt;
```

##### 3.2 延迟加载

对于延迟加载的场景，可以使用属性：

```csharp
private List<Order> _orders;
public List<Order> Orders 
{
    get 
    {
        if (_orders == null)
            _orders = LoadOrders(); // 轻量级延迟加载
        return _orders;
    }
}
```

### 规范理由总结

1. **语义清晰**：属性表示"是什么"，方法表示"做什么"
2. **符合约定**：遵循C#社区和微软的编码约定
3. **框架兼容**：更好地支持数据绑定、序列化等框架功能
4. **性能考量**：调用者可以合理假设属性访问是轻量级的
5. **维护性**：清晰的区分有助于代码理解和维护
