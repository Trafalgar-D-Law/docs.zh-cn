---
title: fixed 语句（C# 参考）
ms.date: 05/10/2018
f1_keywords:
- fixed_CSharpKeyword
- fixed
helpviewer_keywords:
- fixed keyword [C#]
ms.openlocfilehash: 021fc3bd63922394bd70495bd4335b068fc51cdd
ms.sourcegitcommit: fb78d8abbdb87144a3872cf154930157090dd933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2018
ms.locfileid: "47237145"
---
# <a name="fixed-statement-c-reference"></a>fixed 语句（C# 参考）

`fixed` 语句可防止垃圾回收器重新定位可移动的变量。 `fixed` 语句仅允许存在于[不安全的](unsafe.md)上下文中。 `fixed` 还可用于创建[固定大小的缓冲区](../../programming-guide/unsafe-code-pointers/fixed-size-buffers.md)。

`fixed` 语句将为托管变量设置一个指针，并在该语句的执行过程中“单边锁定”该变量。 仅可在 `fixed` 上下文中使用指向可移动托管变量的指针。 如果没有 `fixed` 上下文，垃圾回收可能会不可预测地重定位变量。 C# 编译器只允许将指针分配给 `fixed` 语句中的托管变量。

[!code-csharp[Accessing fixed memory](../../../../samples/snippets/csharp/keywords/FixedKeywordExamples.cs#1)]

可以通过使用一个数组、字符串、固定大小的缓冲区或变量的地址来初始化指针。 下面的示例说明了变量地址、数组和字符串的使用。 有关固定大小的缓冲区的详细信息，请参阅[固定大小的缓冲区](../../programming-guide/unsafe-code-pointers/fixed-size-buffers.md)。

[!code-csharp[Initializing fixed size buffers](../../../../samples/snippets/csharp/keywords/FixedKeywordExamples.cs#2)]

从 C# 7.3 开始，`fixed` 语句可在数组、字符串、固定大小缓冲区或非托管变量以外的其他类型上执行。 实施名为 `GetPinnableReference` 的方法的任何类型都可以被固定。 `GetPinnableReference` 必须向非托管类型返回 `ref` 变量。 请参阅有关[指针类型](../../programming-guide/unsafe-code-pointers/pointer-types.md)的主题以了解详细信息。 .NET Core 2.0 中引入的 .NET 类型 <xref:System.Span%601?displayProperty=nameWithType> 和 <xref:System.ReadOnlySpan%601?displayProperty=nameWithType> 使用此模式，并且可以固定。 下面的示例对此进行了演示：

[!code-csharp[Accessing fixed memory](../../../../samples/snippets/csharp/keywords/FixedKeywordExamples.cs#FixedSpan)]

如果正在创建应加入此模式的类型，请参阅 <xref:System.Span%601.GetPinnableReference?displayProperty=nameWithType> 以查看有关实施此模式的示例。

如果它们都是同一类型，则可以在一个语句中初始化多个指针：

```csharp
fixed (byte* ps = srcarray, pd = dstarray) {...}
```

若要初始化不同类型的指针，只需嵌套 `fixed` 语句，如下面的示例中所示。

[!code-csharp[Initializing multiple pointers](../../../../samples/snippets/csharp/keywords/FixedKeywordExamples.cs#3)]

执行该语句中的代码之后，任何固定的变量都将被解锁并受垃圾回收的约束。 因此，请勿指向 `fixed` 语句之外的那些变量。 在 `fixed` 语句中声明的变量的作用域为该语句，使此操作更容易：

```csharp
fixed (byte* ps = srcarray, pd = dstarray)
{
   ...
}
// ps and pd are no longer in scope here.
```

在 `fixed` 语句中初始化的指针为只读变量。 如果想要修改指针值，必须声明第二个指针变量，并修改它。 不能修改在 `fixed` 语句中声明的变量：

```csharp
fixed (byte* ps = srcarray, pd = dstarray)
{
    byte* pSourceCopy = ps;
    pSourceCopy++; // point to the next element.
    ps++; // invalid: cannot modify ps, as it is declared in the fixed statement.
}
```


在不安全模式中，可以在栈上分配内存，在这种情况下，内存不受垃圾回收的约束，因此不需要固定。 有关详细信息，请参阅 [stackalloc](stackalloc.md)。

[!code-csharp[Initializing multiple pointers](../../../../samples/snippets/csharp/keywords/FixedKeywordExamples.cs#4)]

## <a name="c-language-specification"></a>C# 语言规范

 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)  
- [C# 编程指南](../../programming-guide/index.md)  
- [C# 关键字](index.md)  
- [unsafe](unsafe.md)  
- [固定大小的缓冲区](../../programming-guide/unsafe-code-pointers/fixed-size-buffers.md)
