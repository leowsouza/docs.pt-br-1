---
title: '- Operadores - e -= – referência do C#'
ms.custom: seodec18
ms.date: 05/27/2019
f1_keywords:
- -_CSharpKeyword
- -=_CSharpKeyword
helpviewer_keywords:
- subtraction operator [C#]
- delegate removal [C#]
- '- operator [C#]'
- subtraction assignment operator [C#]
- event unsubscription [C#]
- -= operator [C#]
ms.assetid: 4de7a4fa-c69d-48e6-aff1-3130af970b2d
ms.openlocfilehash: cf642fcac7233d27f2ed9052829c145038e93419
ms.sourcegitcommit: ad800f019ac976cb669e635fb0ea49db740e6890
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73038884"
---
# <a name="--and---operators-c-reference"></a>Operadores - e -= (referência do C#)

Os operadores de `-` e `-=` são suportados pelos tipos numéricos [internos e de](../builtin-types/integral-numeric-types.md) [ponto flutuante](../builtin-types/floating-point-numeric-types.md) e tipos [delegados](../builtin-types/reference-types.md#the-delegate-type) .

Para obter informações sobre o operador `-` aritmético, consulte as seções [Operadores de adição e subtração unários](arithmetic-operators.md#unary-plus-and-minus-operators) e [Operador de subtração -](arithmetic-operators.md#subtraction-operator--) do artigo [Operadores aritméticos](arithmetic-operators.md).

## <a name="delegate-removal"></a>Remoção de delegado

Para operandos do mesmo tipo [delegado](../builtin-types/reference-types.md#the-delegate-type), o operador `-` retorna uma instância de delegado que é calculada da seguinte maneira:

- Se ambos os operandos forem não nulos e a lista de invocação do operando à direita for uma sublista contígua apropriada da lista de invocação do operando à esquerda, o resultado da operação será uma nova lista de invocação obtida removendo-se as entradas do operando à direita da lista de invocação do operando à esquerda. Se a lista do operando à direita corresponder a várias sublistas contíguas na lista do operando à esquerda, somente a sublista correspondente mais à direita será removida. Se a remoção resultar em uma lista vazia, o resultado será `null`.

  [!code-csharp-interactive[delegate removal](~/samples/csharp/language-reference/operators/SubtractionOperator.cs#DelegateRemoval)]

- Se a lista de invocação do operando à direita não for uma sublista contígua apropriada da lista de invocação do operando à esquerda, o resultado da operação será o operando à esquerda. Por exemplo, a remoção de um delegado que não faz parte do delegado multicast não tem consequências, e o delegado multicast permanece inalterado.

  [!code-csharp-interactive[delegate removal with no effect](~/samples/csharp/language-reference/operators/SubtractionOperator.cs#DelegateRemovalNoChange)]

  O exemplo anterior também demonstra que, durante a remoção de delegados, as instâncias de delegado são comparadas. Por exemplo, delegados produzidos pela avaliação de [expressões lambda](../../programming-guide/statements-expressions-operators/lambda-expressions.md) idênticas não são iguais. Para saber mais sobre a igualdade de delegados, confira a seção [Operadores de igualdade de delegados](~/_csharplang/spec/expressions.md#delegate-equality-operators) da [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

- Se o operando à esquerda for `null`, o resultado da operação será `null`. Se o operando à direita for `null`, o resultado da operação será o operando à esquerda.

  [!code-csharp-interactive[delegate removal and null](~/samples/csharp/language-reference/operators/SubtractionOperator.cs#DelegateRemovalAndNull)]

Para combinar delegados, use o [operador `+`](addition-operator.md#delegate-combination).

Para obter mais informações sobre tipos de delegado, veja [Delegados](../../programming-guide/delegates/index.md).

## <a name="subtraction-assignment-operator--"></a>Operador de atribuição de subtração -=

Uma expressão que usa o operador `-=`, como

```csharp
x -= y
```

equivale a

```csharp
x = x - y
```

exceto que `x` é avaliado apenas uma vez.

O exemplo a seguir demonstra o uso do operador `-=`:

[!code-csharp-interactive[-= examples](~/samples/csharp/language-reference/operators/SubtractionOperator.cs#SubtractAndAssign)]

Você também usará o operador `-=` para especificar um método de manipulador de eventos a remover ao cancelar a assinatura de um [evento](../keywords/event.md). Para obter mais informações, confira [Como assinar e cancelar a assinatura de eventos](../../programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events.md).

## <a name="operator-overloadability"></a>Capacidade de sobrecarga do operador

Um tipo definido pelo usuário pode [sobrecarregar](operator-overloading.md) o operador `-`. Quando um operador `-` binário é sobrecarregado, o operador `-=` também é implicitamente sobrecarregado. Um tipo definido pelo usuário não pode sobrecarregar explicitamente o operador `-=`.

## <a name="c-language-specification"></a>Especificação da linguagem C#

Para obter mais informações, veja as seções [Operador de subtração unário](~/_csharplang/spec/expressions.md#unary-minus-operator) e [Operador de subtração](~/_csharplang/spec/expressions.md#subtraction-operator) da [Especificação de linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores do C#](index.md)
- [Eventos](../../programming-guide/events/index.md)
- [Operadores aritméticos](arithmetic-operators.md)
- [Operadores + e +=](addition-operator.md)
