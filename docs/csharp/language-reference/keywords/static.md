---
title: Modificador static – Referência de C#
ms.custom: seodec18
ms.date: 07/20/2015
f1_keywords:
- static
- static_CSharpKeyword
helpviewer_keywords:
- static keyword [C#]
ms.assetid: 5509e215-2183-4da3-bab4-6b7e607a4fdf
ms.openlocfilehash: cbd0f6b4ef7976ccc2da2a735ccbba2bf23177e4
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2019
ms.locfileid: "73422326"
---
# <a name="static-c-reference"></a>static (Referência de C#)

Use o modificador `static` para declarar um membro estático que pertença ao próprio tipo, em vez de um objeto específico. O modificador `static` pode ser usado com classes, campos, métodos, propriedades, operadores, eventos e construtores, mas não pode ser usado com indexadores, finalizadores ou tipos diferentes de classes. Para obter mais informações, consulte [Classes Estáticas e Membros de Classes Estáticas](../../programming-guide/classes-and-structs/static-classes-and-static-class-members.md).

## <a name="example"></a>Exemplo

A seguinte classe é declarada como `static` e contém apenas métodos `static`:

[!code-csharp[csrefKeywordsModifiers#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#18)]

Uma constante ou declaração de tipo é, implicitamente, um membro estático.

Um membro estático não pode ser referenciado por meio de uma instância. Em vez disso, ele é referenciado pelo nome do tipo. Por exemplo, considere a seguinte classe:

[!code-csharp[csrefKeywordsModifiers#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#19)]

Para fazer referência ao membro estático `x`, use o nome totalmente qualificado `MyBaseC.MyStruct.x`, a menos que o membro esteja acessível no mesmo escopo:

```csharp
Console.WriteLine(MyBaseC.MyStruct.x);
```

Embora uma instância de uma classe contenha uma cópia separada de todos os campos de instância da classe, há apenas uma cópia de cada campo estático.

Não é possível usar [this](this.md) para referenciar métodos estáticos ou acessadores de propriedade.

Se a palavra-chave `static` for aplicada a uma classe, todos os membros da classe deverão ser estáticos.

As classes e as classes estáticas podem ter construtores estáticos. Os construtores estáticos são chamados em algum ponto entre o momento em que o programa é iniciado e a classe é instanciada.

> [!NOTE]
> A palavra-chave `static` tem utilizações mais limitadas do que no C++. Para comparar com a palavra-chave do C++, consulte [Storage classes (C++)](/cpp/cpp/storage-classes-cpp#static) (Classes de armazenamento (C++)).

Para demonstrar os membros estáticos, considere uma classe que representa um funcionário da empresa. Suponha que a classe contém um método para contar funcionários e um campo para armazenar o número de funcionários. O método e o campo não pertencem a qualquer instância funcionário. Em vez disso, eles pertencem à classe empresa. Portanto, eles devem ser declarados como membros estáticos da classe.

## <a name="example"></a>Exemplo

Este exemplo lê o nome e a ID de um novo funcionário, incrementa o contador de funcionário em um e exibe as informações do novo funcionário e do novo número de funcionários. Para simplificar, este programa lê, do teclado, o número atual de funcionários. Em um aplicativo real, essas informações devem ser lidas de um arquivo.

[!code-csharp[csrefKeywordsModifiers#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#20)]  

## <a name="example"></a>Exemplo

Este exemplo mostra que, embora seja possível inicializar um campo estático usando outro campo estático que ainda não foi declarado, os resultados serão indefinidos até que você atribua explicitamente um valor ao campo estático.

[!code-csharp[csrefKeywordsModifiers#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#21)]  

## <a name="c-language-specification"></a>Especificação da linguagem C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Guia de Programação em C#](../../programming-guide/index.md)
- [Palavras-chave do C#](index.md)
- [Modificadores](index.md)
- [Classes static e membros de classes static](../../programming-guide/classes-and-structs/static-classes-and-static-class-members.md)
