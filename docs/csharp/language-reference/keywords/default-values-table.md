---
title: Tabela de valores padrão – Referência de C#
ms.custom: seodec18
description: Saiba quais são os valores padrão dos tipos C#.
ms.date: 07/29/2019
helpviewer_keywords:
- default [C#]
- parameterless constructor [C#]
ms.openlocfilehash: 48aa294fa9e37e2e138444e493faa5474011097e
ms.sourcegitcommit: 93762e1a0dae1b5f64d82eebb7b705a6d566d839
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74551817"
---
# <a name="default-values-table-c-reference"></a>Tabela de valores padrão (referência de C#)

A seguinte tabela mostra os valores padrão de tipos C#:

|{1&gt;Tipo&lt;1}|Valor padrão|
|---------|------------------|
|Qualquer tipo de referência|`null`|
|Qualquer [tipo numérico integral interno](../builtin-types/integral-numeric-types.md)|0 (zero)|
|Qualquer [tipo numérico de ponto flutuante interno](../builtin-types/floating-point-numeric-types.md)|0 (zero)|
|[bool](../builtin-types/bool.md)|`false`|
|[char](../builtin-types/char.md)|`'\0'` (U+0000)|
|[enum](enum.md)|O valor é produzido pela expressão `(E)0`, em que `E` é o identificador de enumeração.|
|[struct](struct.md)|O valor produzido pela configuração de todos os campos tipo-valor para seus valores padrão e todos os campos tipo-referência para `null`.|
|Qualquer [tipo de valor que permite valor nulo](../builtin-types/nullable-value-types.md)|Uma instância para a qual a propriedade <xref:System.Nullable%601.HasValue%2A> é `false` e a propriedade <xref:System.Nullable%601.Value%2A> não está definida. Esse valor padrão também é conhecido como o valor *nulo* de um tipo de valor anulável.|

Use o [operador padrão](../operators/default.md) para produzir o valor padrão de um tipo, como mostra o exemplo a seguir:

```csharp
int a = default(int);
```

Começando no C# 7.1, você pode usar o [`default` literal](../operators/default.md#default-literal) para inicializar uma variável com o valor padrão de seu tipo:

```csharp
int a = default;
```

Para um tipo de valor, o construtor implícito sem parâmetros também produz o valor padrão do tipo, como mostra o seguinte exemplo:

```csharp-interactive
var n = new System.Numerics.Complex();
Console.WriteLine(n);  // output: (0, 0)
```

## <a name="c-language-specification"></a>Especificação da linguagem C#

Para obter mais informações, confira as seguintes seções da [especificação da linguagem C#](~/_csharplang/spec/introduction.md):

- [Valores padrão](~/_csharplang/spec/variables.md#default-values)
- [Construtores padrão](~/_csharplang/spec/types.md#default-constructors)

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Palavras-chave do C#](index.md)
- [Tabela de tipos internos](built-in-types-table.md)
- [Construtores](../../programming-guide/classes-and-structs/constructors.md)
