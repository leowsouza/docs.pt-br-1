---
title: Diretiva using static – Referência de C#
ms.custom: seodec18
ms.date: 03/10/2017
helpviewer_keywords:
- using static directive [C#]
ms.assetid: 8b8f9e34-c75e-469b-ba85-6f2eb4090314
ms.openlocfilehash: 1a0e26d8b0a14e0c77b724fc492588e08762e47f
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73099991"
---
# <a name="using-static-directive-c-reference"></a>Diretiva using static (Referência de C#)

A diretiva `using static` designa um tipo cujos membros estáticos e tipos aninhados você pode acessar sem especificar um nome de tipo. A sintaxe é:

```csharp
using static <fully-qualified-type-name>;
```

em que *fully-qualified-type-name* é o nome do tipo cujos membros estáticos e tipos aninhados podem ser referenciados sem especificar um nome de tipo. Se você não fornecer um nome de tipo totalmente qualificado (o nome do namespace completo juntamente com o nome do tipo), o C# gerará o erro de compilador [CS0246](../compiler-messages/cs0246.md): “O tipo ou o nome do namespace 'type/namespace' não pôde ser encontrado (uma diretiva using ou uma referência de assembly está ausente?)”.

A diretiva `using static` aplica-se a qualquer tipo que tenha membros estático (ou tipos aninhados), mesmo que ele também tenha membros de instância. No entanto, os membros da instância podem ser invocados apenas por meio de instância de tipo.

A diretiva `using static` foi introduzida no C# 6.

## <a name="remarks"></a>Comentários

Normalmente, quando você chamar um membro estático, fornece o nome do tipo juntamente com o nome do membro. Inserir repetidamente o mesmo nome de tipo para invocar os membros do tipo pode resultar em código obscuro detalhado. Por exemplo, a seguinte definição de uma classe `Circle` faz referência a um número de membros da classe <xref:System.Math>.

[!code-csharp[using-static#1](~/samples/snippets/csharp/language-reference/keywords/using/using-static1.cs#1)]

Eliminando a necessidade de fazer referência explícita à classe <xref:System.Math> sempre que um membro é referenciado, a diretiva `using static` produz um código muito mais limpo:

[!code-csharp[using-static#2](~/samples/snippets/csharp/language-reference/keywords/using/using-static2.cs#1)]

`using static` importa somente os membros estáticos acessíveis e os tipos aninhados declarados no tipo especificado.  Os membros herdados não são importados.  Você pode importar de qualquer tipo nomeado com uma diretiva using static, incluindo módulos do Visual Basic.  Se funções de nível superior do F# aparecerem nos metadados como membros estáticos de um tipo nomeado cujo nome é um identificador válido do C#, as funções do F# poderão ser importadas.

 `using static` torna os métodos de extensão declarados no tipo especificado disponível para pesquisa de método de extensão.  No entanto, os nomes dos métodos de extensão não são importados no escopo para a referência não qualificada no código.

 Métodos com o mesmo nome importados de diferentes tipos por diferentes diretivas `using static` na mesma unidade de compilação ou namespace formam um grupo de métodos.  A resolução de sobrecarga nesses grupos de métodos segue as regras normais de C#.

## <a name="example"></a>Exemplo

O exemplo a seguir usa a diretiva `using static` para tornar os membros estáticos das classes <xref:System.Console>, <xref:System.Math> e <xref:System.String> disponíveis sem a necessidade de especificar seu nome de tipo.

[!code-csharp[using-static#3](~/samples/snippets/csharp/language-reference/keywords/using/using-static3.cs)]

No exemplo, a diretiva `using static` também poderia ter sido aplicada ao tipo <xref:System.Double>. Isso tornaria possível chamar o método <xref:System.Double.TryParse(System.String,System.Double@)> sem especificar um nome de tipo. No entanto, isso cria código menos legível, uma vez que se torna necessário verificar as instruções `using static` para determinar qual método `TryParse` do tipo numérico é chamado.

## <a name="see-also"></a>Consulte também

- [Diretiva using](using-directive.md)
- [Referência de C#](../index.md)
- [Palavras-chave do C#](index.md)
- [Usando namespaces](../../programming-guide/namespaces/using-namespaces.md)
- [Namespaces](../../programming-guide/namespaces/index.md)
