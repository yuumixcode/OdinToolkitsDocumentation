---
date: 2025-10-17
categories:
  - manuals
tags:
  - ScriptDocGenerator
authors: [Yuumix]
slug: type-analyzer-unit-test-overview
---
# TypeAnalyzer 支持情况单元测试总览

## Type 解析支持单元测试

!!! note "Type 解析支持单元测试须知"

    类型解析数据（TypeData）进行单元测试的类型一共有 14 种
    
    类型解析支持包括类、结构体、枚举、接口、委托、嵌套类型、记录类。

---

??? success "完美支持部分类型，共 14 种"

    ``` csharp
    public interface ITestInterface { }
    [Summary("支持解析特性")]
    [ReferenceLinkURL("https://learn.microsoft.com/en-us/dotnet/api/system.object?view=net-9.0")]
    public class TestClassWithAttribute { }
    public abstract class TestAbstractClass { }
    public sealed class TestSealedClass { }
    public static class TestStaticClass { }
    public class TestGenericClass<T> where T : class
    {
        public T Owner;
    }
    public delegate void TestDelegate();
    public delegate void TestDelegateHasParameters(int a, List<string> b);
    public delegate bool TestDelegateHasReturnType(float a, int[] b);
    public record TestRecord;
    public struct TestStruct { }
    public class UnitTestTypes : TestAbstractClass { }
    ```

    | 解析后的类型完整声明 - 单行 |
    | :---------- |
    | `public interface ITestInterface` |
    | `public abstract class TestAbstractClass` |
    | `public sealed class TestSealedClass` |
    | `public static class TestStaticClass` |
    | `public class TestGenericClass<T> where T : class` |
    | `public delegate void TestDelegate()` |
    | `public delegate void TestDelegateHasParameters(int a, List<string> b)` |
    | `public delegate bool TestDelegateHasReturnType(float a, int[] b)` |

    ``` markdown title="TestClassWithAttribute 的完整声明"
    [ReferenceLinkURL]
    public class TestClassWithAttribute
    ```

    ``` markdown title="TestRecord 的完整声明"
    [NullableContext(NullableContextOptions.flow)]
    [Nullable(NullableContextOptions.flow)]
    public record TestRecord : System.IEquatable<TestRecord>
    ```

    ``` markdown title="TestStruct 的完整声明"
    public struct TestStruct : System.ValueType
    ```

    ``` markdown title="UnitTestTypes 的完整声明"
    public class UnitTestTypes : Yuumix.OdinToolkits.Tests.Editor.TestAbstractClass
    ```

---

## 构造方法（Construtor）解析支持单元测试

!!! note "构造方法解析支持单元测试须知"

    构造方法是类的一种特殊方法，用于创建类的实例。构造方法的名称与类名相同，没有返回值类型。

---

??? success "完美支持构造方法，共 4 种"

    ``` csharp
    public abstract class TestClassAbstract
    {
        protected TestClassAbstract(int a) { }
        protected TestClassAbstract() { }
    }
    public class TestClass : TestClassAbstract
    {
        public TestClass() { }
        public TestClass(bool b, int a) : base(a) { }
        static TestClass() { }
        TestClass(string s) { }
    }
    ```

    | 解析后的构造方法完整签名 |
    | :---------- |
    | `public UnitTestConstructorsCommon.TestClass()` |
    | `public UnitTestConstructorsCommon.TestClass(bool b, int a)` |
    | `private static UnitTestConstructorsCommon.TestClass()` |
    | `private UnitTestConstructorsCommon.TestClass(string s)` |

---

## 事件（Event）解析支持单元测试

!!! note "事件解析支持单元测试须知"

    只解析使用 `event` 关键字声明的事件，简单委托属于字段。

    事件相当于在字段的声明中添加了 `event` 关键字，解析支持参考字段解析支持。

    事件解析数据（EventData）进行单元测试的事件一共有 6 种

---

### 静态和实例事件解析支持

??? success "完美支持静态和实例事件，共 6 种"

    ``` csharp
    public event Action ActionEvent;
    public event Action<int, string> ActionWithParamsEvent;
    public event Func<int, string, bool> FuncWithParamsEvent;
    public event Predicate<int> PredicateEvent;
    public event Comparison<string> ComparisonEvent;
    public static event Action<bool> StaticActionEvent;
    ```

    | 解析后的事件完整签名 |
    | :---------- |
    | `public event Action ActionEvent;` |
    | `public event Action<int, string> ActionWithParamsEvent;` |
    | `public event Func<int, string, bool> FuncWithParamsEvent;` |
    | `public event Predicate<int> PredicateEvent;` |
    | `public event Comparison<string> ComparisonEvent;` |
    | `public static event Action<bool> StaticActionEvent;` |

---

## 方法（Method）解析支持单元测试

!!! note "方法解析支持单元测试须知"

    方法解析数据（MethodData）进行单元测试的方法一共有 23 种。

    方法解析数据不包括构造函数，仅包括普通方法。

---

### 普通方法解析支持

??? success "完美支持普通方法，共 11 种"

    ``` csharp
    public void EmptyParamMethod()
    {
        Debug.Log("EmptyParamMethod");
    }

    public void OneIntParamMethod(int param)
    {
        Debug.Log(param);
    }

    public void TwoParamMethod(string param1, bool param2)
    {
        Debug.Log(param1 + param2);
    }

    public void GenericMethod<T>(T param)
    {
        Debug.Log(param);
    }

    public void TwoParamsGenericMethod<T, T1>(T param1, List<T1> param2)
    {
        Debug.Log(param1);
        Debug.Log(param2);
    }

    public void IndefiniteParamsMethod(string param1, params bool[] param2)
    {
        Debug.Log(param1 + param2);
    }

    public string HasReturnStringMethod(float param1)
    {
        Debug.Log(param1);
        return "HasReturnStringMethod";
    }

    public static void StaticMethod()
    {
        Debug.Log("StaticMethod");
    }

    public static bool HasReturnBoolMethodWithDefault(float param1 = 5f, bool param2 = false,
        string param3 = "Hello World", int param4 = 0,
        ScriptDocGeneratorTestEnum param5 = ScriptDocGeneratorTestEnum.Value3)
    {
        Debug.Log(param3 + param4);
        return true;
    }

    public static async Task AsyncMethod()
    {
        await Task.Delay(1);
    }

    public async Task<int> AsyncMethodWithReturnValue()
    {
        await Task.Delay(1);
        return 42;
    }
    ```

    | 解析后的方法完整签名 |
    | :---------- |
    | `public void EmptyParamMethod()` |
    | `public void OneIntParamMethod(int param)` |
    | `public void TwoParamMethod(string param1, bool param2)` |
    | `public void GenericMethod<T>(T param)` |
    | `public void TwoParamsGenericMethod<T, T1>(T param1, List<T1> param2)` |
    | `public void IndefiniteParamsMethod(string param1, params bool[] param2)` |
    | `public string HasReturnStringMethod(float param1)` |
    | `public static void StaticMethod()` |
    | `public static bool HasReturnBoolMethodWithDefault(float param1 = 5f, bool param2 = false, string param3 = "Hello World", int param4 = 0, ScriptDocGeneratorTestEnum param5 = ScriptDocGeneratorTestEnum.Value3)` |
    | `public static async Task AsyncMethod()` |
    | `public async Task<int> AsyncMethodWithReturnValue()` |

### 继承以及接口方法解析支持

??? success "完美支持继承以及接口方法，共 4 种"

    ``` csharp
    public interface IInterface
    {
        void InterfaceMethod();
    }

    public abstract class TestClassAbstract
    {
        public virtual void OverrideVirtualMethod() { }
        public abstract void OverrideAbstractMethod();
    }

    public class TestClassImplement : TestClassAbstract, IInterface
    {
        public override void OverrideAbstractMethod() { }
        public override void OverrideVirtualMethod() { 
        public void InterfaceMethod()
        {
            Debug.Log("InterfaceMethod");
        }
    }
    ```

    | 解析后的方法完整签名 |
    | :---------- |

    | `public virtual void OverrideVirtualMethod()` |
    | `public override void OverrideAbstractMethod()` |
    | `public override void OverrideVirtualMethod()` |
    | `public void InterfaceMethod()` |

### 运算符方法解析支持

??? success "完美支持运算符方法，共 7 种"

    ``` csharp
    public static TestClass operator +(TestClass a, TestClass b) => new TestClass();
    public static TestClass operator -(TestClass a, TestClass b) => new TestClass();
    public static TestClass operator *(TestClass a, TestClass b) => new TestClass();
    public static TestClass operator /(TestClass a, TestClass b) => new TestClass();
    public static TestClass operator %(TestClass a, TestClass b) => new TestClass();
    public static implicit operator TestClass(int a) => new TestClass();
    public static explicit operator float(TestClass a) => 1f;
    ```

    | 解析后的运算符方法完整签名 |
    | :---------- |
    | `public static UnitTestMethodsOperator.TestClass operator +(UnitTestMethodsOperator.TestClass a, UnitTestMethodsOperator.TestClass b)` |
    | `public static UnitTestMethodsOperator.TestClass operator -(UnitTestMethodsOperator.TestClass a, UnitTestMethodsOperator.TestClass b)` |
    | `public static UnitTestMethodsOperator.TestClass operator *(UnitTestMethodsOperator.TestClass a, UnitTestMethodsOperator.TestClass b)` |
    | `public static UnitTestMethodsOperator.TestClass operator /(UnitTestMethodsOperator.TestClass a, UnitTestMethodsOperator.TestClass b)` |
    | `public static UnitTestMethodsOperator.TestClass operator %(UnitTestMethodsOperator.TestClass a, UnitTestMethodsOperator.TestClass b)` |
    | `public static implicit operator UnitTestMethodsOperator.TestClass(int a)` |
    | `public static explicit operator float(UnitTestMethodsOperator.TestClass a)` |

### 静态扩展方法解析支持

??? success "完美支持静态扩展方法，共 1 种"

    ``` csharp
    public static class TestStaticExtension
    {
        public static int StaticMethod(this UnitTestMethodsStaticExtension.TestClass t) => 0;
    }
    ```

    | 解析后的静态扩展方法完整签名 |
    | :---------- |
    | `public static int StaticMethod(this UnitTestMethodsStaticExtension.TestClass t)` |

---

## 属性（Property）解析支持单元测试

!!! note "属性解析支持单元测试须知"

    属性解析数据（PropertyData）进行单元测试的属性一共有 19 种。

    属性相当于在字段的声明中添加了 `get` 和 `set` 访问器，解析支持参考字段解析支持。

    属性自定义默认值解析支持包括两种情况：

        1. 在属性声明时直接赋值
   
        2. 在无参数构造方法中赋值，必须是无参数构造方法

!!! danger "不支持属性的访问修饰符与 get 和 set 访问器都不同的情况"

    ``` csharp
    protected bool BoolPropertyPrivateGetPublicSet { private get; set; }
    ```

---

### 实例和静态基础类型属性解析支持

??? success "完美支持属性，共 8 种"

    ``` csharp
    public int IntPropertyPublicGetPublicSet { get; set; }
    public string StringPropertyPublicGetInternalSet { get; internal set; }
    public float FloatPropertyPublicGetProtectedSet { get; protected set; }
    public bool BoolPropertyPublicGetPrivateSet { get; private set; }
    public int IntPropertyInternalGetPublicSet { internal get; set; }
    public float FloatPropertyProtectedGetPublicSet { protected get; set; }
    public bool BoolPropertyPrivateGetPublicSet { private get; set; }
    public static int StaticIntPropertyPublicGetPublicSet { get; set; }
    ```

    | 解析后的属性完整签名 |
    | :---------- |
    | `public int IntPropertyPublicGetPublicSet { get; set; }` |
    | `public string StringPropertyPublicGetInternalSet { get; internal set; }` |
    | `public float FloatPropertyPublicGetProtectedSet { get; protected set; }` |
    | `public bool BoolPropertyPublicGetPrivateSet { get; private set; }` |
    | `public int IntPropertyInternalGetPublicSet { internal get; set; }` |
    | `public float FloatPropertyProtectedGetPublicSet { protected get; set; }` |
    | `public bool BoolPropertyPrivateGetPublicSet { private get; set; }` |
    | `public static int StaticIntPropertyPublicGetPublicSet { get; set; }` |

### 自定义默认值属性解析支持

??? success "完美支持自定义默认值属性，共 11 种"

    ``` csharp
    public static int StaticIntPropertyWithDefaultValue { get; set; } = 1;
    public static float StaticFloatPropertyWithDefaultValue { get; set; } = 1f;
    public static bool StaticBoolPropertyWithDefaultValue { get; set; } = true;
    public static string StaticStringPropertyWithDefaultValue { get; set; } = "Hello";
    public static TestEnum StaticEnumPropertyWithDefaultValue { get; set; } = TestEnum.B;
    public int IntPropertyWithDefaultValue { get; internal set; } = 77;
    public float FloatPropertyWithDefaultValue { get; protected set; } = 77f;
    public bool BoolPropertyWithDefaultValue { get; private set; } = true;
    public string StringPropertyWithDefaultValue { get; set; } = "World";
    public TestEnum EnumPropertyWithDefaultValue { get; set; } = TestEnum.C;
    public string StringPropertyInitOnCtor { get; set; }
    public TestClass()
    {
        StringPropertyInitOnCtor = "Hello World";
    }
    ```

    | 解析后的属性完整签名 |
    | :---------- |
    | `public static int StaticIntPropertyWithDefaultValue { get; set; } = 1;` |
    | `public static float StaticFloatPropertyWithDefaultValue { get; set; } = 1f;` |
    | `public static bool StaticBoolPropertyWithDefaultValue { get; set; } = true;` |
    | `public static string StaticStringPropertyWithDefaultValue { get; set; } = "Hello";` |
    | `public static TestEnum StaticEnumPropertyWithDefaultValue { get; set; } = TestEnum.B;` |
    | `public int IntPropertyWithDefaultValue { get; internal set; } = 77;` |
    | `public float FloatPropertyWithDefaultValue { get; protected set; } = 77f;` |
    | `public bool BoolPropertyWithDefaultValue { get; private set; } = true;` |
    | `public string StringPropertyWithDefaultValue { get; set; } = "World";` |
    | `public TestEnum EnumPropertyWithDefaultValue { get; set; } = TestEnum.C;` |
    | `public string StringPropertyInitOnCtor { get; set; } = "Hello World";` |

---

## 字段（Field）解析支持单元测试

!!! note "字段解析支持单元测试须知"

    字段解析数据（FieldData）进行单元测试的字段一共有 101 种

---

### 不同访问修饰符和复合关键字解析支持

??? success "完美支持不同访问修饰符，共 6 种"

    ``` csharp
    int _privateField;
    internal int InternalField;
    private protected int PrivateProtectedField;
    protected int ProtectedField;
    protected internal int ProtectedInternalField;
    public int PublicField;
    ```

    | 解析后的字段完整签名 |
    | :----------|
    | `private int _privateField;` |
    | `internal int InternalField;` |
    | `private protected int PrivateProtectedField;` |
    | `protected int ProtectedField;` |
    | `protected internal int ProtectedInternalField;` |
    | `public int PublicField;` |

??? success "完美支持复合关键字，共 4 种"

    ``` csharp
    public const int CONST_FIELD = 42;
    public static readonly int StaticReadOnlyField;
    public static int StaticField;
    public readonly int ReadOnlyField;
    ```

    | 解析后的字段完整签名 |
    | :---------- |
    | `public const int CONST_FIELD = 42;` |
    | `public static readonly int StaticReadOnlyField;` |
    | `public static int StaticField;` |
    | `public readonly int ReadOnlyField;` |

### 常量字段解析支持

!!! tip "常量字段解析须知"

    1. 常量字段自定义设置默认值的解析测试只包括基础值类型、枚举类型、特殊的 string 引用类型。
   
    2. 常量字段部分类型解析限制如下：

        a. 长整型常量字段，long，解析后的字段数据设置为以 'L' 字符结尾。

        b. 无符号长整型常量字段，ulong，解析后的字段数据设置为以 'ul' 字符结尾。

        c. 无符号整型常量字段，uint，解析后的字段数据设置为以 'u' 字符结尾。

        d. 双精度浮点型常量字段，double，解析后的字段数据设置为以 'd' 字符结尾，且为了保证精准，位数不能超过 15 位，这里的 15 位是指有效数字的位数，不只是小数点后的位数。

        e. 十进制浮点型常量字段，decimal，解析后的字段数据设置为以 'm' 字符结尾。

        f. 嵌套类枚举常量字段，UnitTestFieldsIsConstantWithDefaultValue.TestEnum，解析后的字段数据的字段类型会显示嵌套路径。

??? success "完美支持大部分常量字段，共 9 种"

    ``` csharp
    public const string STRING_CONST_FIELD = "Hello, World!";
    public const int INT_CONST_FIELD = 2147483647;
    public const float FLOAT_CONST_FIELD = 3.14159f;
    public const bool BOOLEAN_CONST_FIELD = true;
    public const char CHAR_CONST_FIELD = 'A';
    public const byte BYTE_CONST_FIELD = 255;
    public const sbyte SBYTE_CONST_FIELD = -128;
    public const short SHORT_CONST_FIELD = 32767;
    public const ushort USHORT_CONST_FIELD = 65535;
    ```

    | 解析后的字段完整签名 |
    | :---------- |
    | `public const string STRING_CONST_FIELD = "Hello, World!";` |
    | `public const int INT_CONST_FIELD = 2147483647;` |
    | `public const float FLOAT_CONST_FIELD = 3.14159f;` |
    | `public const bool BOOLEAN_CONST_FIELD = true;` |
    | `public const char CHAR_CONST_FIELD = 'A';` |
    | `public const byte BYTE_CONST_FIELD = 255;` |
    | `public const sbyte SBYTE_CONST_FIELD = -128;` |
    | `public const short SHORT_CONST_FIELD = 32767;` |
    | `public const ushort USHORT_CONST_FIELD = 65535;` |

??? warning "有限制的支持部分常量字段，共 7 种"

    ``` csharp
    public const long LONG_CONST_FIELD = 9223372036854775807L;
    public const ulong ULONG_CONST_FIELD = 18446744073709551615ul;
    public const uint UINT_CONST_FIELD = 4294967295u;
    public const double DOUBLE_CONST_FIELD = 2.71828182845904d; 
    public const decimal DECIMAL_CONST_FIELD = 123.456m;
    public const ScriptDocGeneratorTestEnum ENUM_CONST_FIELD = ScriptDocGeneratorTestEnum.Value1;
    
    // 嵌套类枚举常量字段，UnitTestFieldsIsConstantWithDefaultValue.TestEnum
    public const TestEnum NESTED_ENUM_CONST_FIELD = TestEnum.Value3;
    ```

    | 解析后的字段完整签名 |
    | :---------- |
    | `public const long LONG_CONST_FIELD = 9223372036854775807L;` |
    | `public const ulong ULONG_CONST_FIELD = 18446744073709551615ul;` |
    | `public const uint UINT_CONST_FIELD = 4294967295u;` |
    | `public const double DOUBLE_CONST_FIELD = 2.71828182845904d;` |
    | `public const decimal DECIMAL_CONST_FIELD = 123.456m;` |
    | `public const ScriptDocGeneratorTestEnum ENUM_CONST_FIELD = ScriptDocGeneratorTestEnum.Value1;` |
    | `public const UnitTestFieldsIsConstantWithDefaultValue.TestEnum NESTED_ENUM_CONST_FIELD = TestEnum.Value3;` |

### 静态字段解析支持

!!! tip "静态字段解析须知"

    1. 静态字段自定义设置默认值的解析测试只包括基础值类型、枚举类型、特殊的 string 引用类型。

    2. 静态字段部分类型解析限制和常量一致

??? success "完美支持大部分的静态字段，共 9 种"

    ``` csharp
    public static string StringStaticField = "Hello, World!";
    public static int INTStaticField = 2147483647;
    public static float FloatStaticField = 3.14159f;
    public static bool BooleanStaticField = true;
    public static char CharStaticField = 'A';
    public static byte ByteStaticField = 255;
    public static sbyte SbyteStaticField = -128;
    public static short ShortStaticField = 32767;
    public static ushort UshortStaticField = 65535;
    ```

    | 解析后的字段完整签名 |
    | :---------- |
    | `public static string StringStaticField = "Hello, World!";` |
    | `public static int INTStaticField = 2147483647;` |
    | `public static float FloatStaticField = 3.14159f;` |
    | `public static bool BooleanStaticField = true;` |
    | `public static char CharStaticField = 'A';` |
    | `public static byte ByteStaticField = 255;` |
    | `public static sbyte SbyteStaticField = -128;` |
    | `public static short ShortStaticField = 32767;` |
    | `public static ushort UshortStaticField = 65535;` |

??? warning "有限制的支持部分静态字段，共 7 种"

    ``` csharp
    public static long LongStaticField = 9223372036854775807L;
    public static ulong UlongStaticField = 18446744073709551615ul;
    public static uint UintStaticField = 4294967295u;
    public static double DoubleStaticField = 2.71828182845904d;
    public static decimal DecimalStaticField = 123.456m;
    public static ScriptDocGeneratorTestEnum EnumStaticField = ScriptDocGeneratorTestEnum.Value2;
    public static TestEnum NestedEnumStaticField = TestEnum.Value3;
    ```

    | 解析后的字段完整签名 |
    | :---------- |
    | `public static long LongStaticField = 9223372036854775807L;` |
    | `public static ulong UlongStaticField = 18446744073709551615ul;` |
    | `public static uint UintStaticField = 4294967295u;` |
    | `public static double DoubleStaticField = 2.71828182845904d;` |
    | `public static decimal DecimalStaticField = 123.456m;` |
    | `public static ScriptDocGeneratorTestEnum EnumStaticField = ScriptDocGeneratorTestEnum.Value2;` |
    | `public static UnitTestFieldsIsStaticWithDefaultValue.TestEnum NestedEnumStaticField = TestEnum.Value3;` |

### 实例字段解析支持

!!! tip "实例字段解析须知"

    1. 实例字段自定义设置默认值的解析测试只包括基础值类型、枚举类型、特殊的 string 引用类型。
    2. 只支持在 ***无参构造函数*** 中进行初始化的值类型字段
    3. 普通实例字段若成功解析，则只读实例字段也可以成功解析，不做另外测试。
    4. 实例字段自定义默认值设置包括：

        1. 直接声明默认值

        2. 在 ***无参构造函数*** 中初始化。

    5. 实例字段部分类型解析限制和常量一致

??? success "完美支持大部分的实例字段，共 18 种"

    ``` csharp
    public string StringField = "Hello, World!";
    public int IntField = 2147483647;
    public float FloatField = 3.14159f;
    public bool BooleanField = true;
    public char CharField = 'A';
    public byte ByteField = 255;
    public sbyte SbyteField = -128;
    public short ShortField = 32767;
    public ushort UshortField = 65535;
    // ---
    public string StringFieldInitOnCtor;
    public int IntFieldInitOnCtor;
    public float FloatFieldInitOnCtor;
    public bool BooleanFieldInitOnCtor;
    public char CharFieldInitOnCtor;
    public byte ByteFieldInitOnCtor;
    public sbyte SbyteFieldInitOnCtor;
    public short ShortFieldInitOnCtor;
    public ushort UshortFieldInitOnCtor;
    // --- 构造方法
    public TestClass()
    {
        StringFieldInitOnCtor = "Initialized in constructor!";
        IntFieldInitOnCtor = -123456789;
        FloatFieldInitOnCtor = -2.71828f;
        BooleanFieldInitOnCtor = true;
        CharFieldInitOnCtor = 'Z';
        ByteFieldInitOnCtor = 128;
        SbyteFieldInitOnCtor = 127;
        ShortFieldInitOnCtor = -32768;
        UshortFieldInitOnCtor = 32768;
        // ...
    }
    ```

    | 解析后的字段完整签名 |
    | :---------- |
    | `public string StringField = "Hello, World!";` |
    | `public int IntField = 2147483647;` |
    | `public float FloatField = 3.14159f;` |
    | `public bool BooleanField = true;` |
    | `public char CharField = 'A';` |
    | `public byte ByteField = 255;` |
    | `public sbyte SbyteField = -128;` |
    | `public short ShortField = 32767;` |
    | `public ushort UshortField = 65535;` |
    | `public string StringFieldInitOnCtor = "Initialized in constructor!";` |
    | `public int IntFieldInitOnCtor = -123456789;` |
    | `public float FloatFieldInitOnCtor = -2.71828f;` |
    | `public bool BooleanFieldInitOnCtor = true;` |
    | `public char CharFieldInitOnCtor = 'Z';` |
    | `public byte ByteFieldInitOnCtor = 128;` |
    | `public sbyte SbyteFieldInitOnCtor = 127;` |
    | `public short ShortFieldInitOnCtor = -32768;` |
    | `public ushort UshortFieldInitOnCtor = 32768;` |

??? warning "有限制的支持部分实例字段，共 14 种"

    ``` csharp
    public long LongField = 9223372036854775807L;
    public ulong UlongField = 18446744073709551615ul;
    public uint UintField = 4294967295u;
    public double DoubleField = 2.71828182845904d;
    public decimal DecimalField = 123.456m;
    public ScriptDocGeneratorTestEnum EnumField = ScriptDocGeneratorTestEnum.Value2;
    public UnitTestFieldsIsStaticWithDefaultValue.TestEnum NestedEnumField = TestEnum.Value2;
    // ---
    public long LongFieldInitOnCtor;
    public ulong UlongFieldInitOnCtor;
    public uint UintFieldInitOnCtor;
    public double DoubleFieldInitOnCtor;
    public decimal DecimalFieldInitOnCtor;
    public ScriptDocGeneratorTestEnum EnumFieldInitOnCtor;
    public UnitTestFieldsIsStaticWithDefaultValue.TestEnum NestedEnumFieldInitOnCtor;
    // --- 构造方法
    public TestClass()
    {
        // ...
        LongFieldInitOnCtor = -9223372036854775808L;
        UlongFieldInitOnCtor = 9223372036854775808ul;
        UintFieldInitOnCtor = 2147483648u;
        DoubleFieldInitOnCtor = -3.14159265358979d;
        DecimalFieldInitOnCtor = -987.654m;
        EnumFieldInitOnCtor = ScriptDocGeneratorTestEnum.Value3;
        NestedEnumFieldInitOnCtor = TestEnum.Value3;
    }
    ```

    | 解析后的字段完整签名 |
    | :---------- |
    | `public long LongField = 9223372036854775807L;` |
    | `public ulong UlongField = 18446744073709551615ul;` |
    | `public uint UintField = 4294967295u;` |
    | `public double DoubleField = 2.71828182845904d;` |
    | `public decimal DecimalField = 123.456m;` |
    | `public ScriptDocGeneratorTestEnum EnumField = ScriptDocGeneratorTestEnum.Value2;` |
    | `public UnitTestFieldsIsStaticWithDefaultValue.TestEnum NestedEnumField = TestEnum.Value2;` |
    | `public long LongFieldInitOnCtor = -9223372036854775808L;` |
    | `public ulong UlongFieldInitOnCtor = 9223372036854775808ul;` |
    | `public uint UintFieldInitOnCtor = 2147483648u;` |
    | `public double DoubleFieldInitOnCtor = -3.14159265358979d;` |
    | `public decimal DecimalFieldInitOnCtor = -987.654m;` |
    | `public ScriptDocGeneratorTestEnum EnumFieldInitOnCtor = ScriptDocGeneratorTestEnum.Value3;` |
    | `public UnitTestFieldsIsStaticWithDefaultValue.TestEnum NestedEnumFieldInitOnCtor = TestEnum.Value3;` |

### 集合字段解析支持

??? success "完美支持解析集合字段，共 16 种"

    ``` csharp
    public int[] ArrayField;
    public int[,] MultiArrayField;
    public int[][] JaggedArrayField;
    public List<string> ListField;
    public Dictionary<string, int> DictionaryField;
    public HashSet<string> HashSetField;
    public SortedDictionary<string, int> SortedDictionaryField;
    public SortedList<string, int> SortedListField;
    public Stack<string> StackField;
    public Queue<int> QueueField;
    public LinkedList<string> LinkedListField;
    public System.Collections.ArrayList ArrayListField;
    public System.Collections.Hashtable HashtableField;
    public IReadOnlyList<string> ReadOnlyListField;
    public IReadOnlyDictionary<string, int> ReadOnlyDictionaryField;
    public ConcurrentDictionary<string, int> ConcurrentDictionaryField;
    ```

    | 解析后的字段完整签名 |
    | :---------- |
    | `public int[] ArrayField;` |
    | `public int[,] MultiArrayField;` |
    | `public int[][] JaggedArrayField;` |
    | `public List<string> ListField;` |
    | `public Dictionary<string, int> DictionaryField;` |
    | `public HashSet<string> HashSetField;` |
    | `public SortedDictionary<string, int> SortedDictionaryField;` |
    | `public SortedList<string, int> SortedListField;` |
    | `public Stack<string> StackField;` |
    | `public Queue<int> QueueField;` |
    | `public LinkedList<string> LinkedListField;` |
    | `public System.Collections.ArrayList ArrayListField;` |
    | `public System.Collections.Hashtable HashtableField;` |
    | `public IReadOnlyList<string> ReadOnlyListField;` |
    | `public IReadOnlyDictionary<string, int> ReadOnlyDictionaryField;` |
    | `public ConcurrentDictionary<string, int> ConcurrentDictionaryField;` |

### Unity 部分特有字段解析支持

??? success "完美支持 Unity 部分特有字段，共 7 种"

    ``` csharp
    public GameObject gameObjectField;
    public Transform transformField;
    public Rigidbody rigidbodyField;
    public Vector3 vector3Field = new Vector3(1, 1, 1);

    [SerializeField]
    [Tooltip("This is a tooltip")]
    [UnityEngine.Range(0, 100)]
    public Quaternion quaternionField = new Quaternion(0, 0, 0, 1);

    [ColorUsage(true, true)]
    public Color colorField = Color.white;

    [Obsolete("Use newField instead")]
    public LayerMask layerMaskField;
    ```

    | 完整声明为单行的字段，包含特性 |
    | :---------- |
    | `public GameObject gameObjectField;` |
    | `public Transform transformField;` |
    | `public Rigidbody rigidbodyField;` |
    | `public Vector3 vector3Field = new Vector3(1.00, 1.00, 1.00);` |

    ``` markdown title="Quaternion 字段解析后的完整声明字符串"
    [SerializeField]
    [UnityEngine.Tooltip("This is a tooltip")]
    [UnityEngine.Range(0, 100)]
    public Quaternion quaternionField = new Quaternion(0.00000, 0.00000, 0.00000, 1.00000);
    ```

    ``` markdown title="Color 字段解析后的完整声明字符串"
    [UnityEngine.ColorUsage(true, true)]
    public Color colorField = RGBA(1.000, 1.000, 1.000, 1.000);
    ```

    ``` markdown title="LayerMask 字段解析后的完整声明字符串"
    [Obsolete("Use newField instead")]
    public LayerMask layerMaskField;
    ```

### 其他杂项字段解析支持

??? success "完美支持其他杂项字段，共 4 种"

    ``` csharp
    public TestAbstractClass AbstractField;
    public dynamic DynamicField;
    public ITestInterface InterfaceField;
    public int? NullableField = null;
    ```

    | 解析后的字段完整签名 |
    | :---------- |
    | `public TestAbstractClass AbstractField;` |
    | `public dynamic DynamicField;` |
    | `public ITestInterface InterfaceField;` |
    | `public int? NullableField = null;` |

---

---
