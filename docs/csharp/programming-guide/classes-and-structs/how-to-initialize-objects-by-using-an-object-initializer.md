---
title: Como inicializar objetos usando um inicializador de objeto – C# guia de programação
ms.custom: seodec18
ms.date: 12/20/2018
helpviewer_keywords:
- object initializers [C#], how to use
- objects [C#], initializing
ms.assetid: 4b75ebb2-2e29-43de-929c-d736a8f27ce6
ms.openlocfilehash: be555688a645c7689e76b5b4499c44255c18dbc8
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73970876"
---
# <a name="how-to-initialize-objects-by-using-an-object-initializer-c-programming-guide"></a>Como inicializar objetos usando um inicializador de objeto (C# guia de programação)

Você pode usar os inicializadores de objeto para inicializar objetos de tipo de maneira declarativa, sem invocar explicitamente um construtor para o tipo.  
  
Os exemplos a seguir mostram como usar os inicializadores de objeto com objetos nomeados. O compilador processa os inicializadores de objetos ao acessar, primeiro, o construtor de instância padrão e, em seguida, processar as inicializações de membro. Portanto, se o construtor sem parâmetros for declarado como `private` na classe, os inicializadores de objeto que exigem acesso público falharão.
  
Se você estiver definindo um tipo anônimo, é necessário usar um inicializador de objeto. Para obter mais informações, consulte [como retornar subconjuntos de propriedades de elemento em uma consulta](how-to-return-subsets-of-element-properties-in-a-query.md).  
  
## <a name="example"></a>Exemplo  

O exemplo a seguir mostra como inicializar um novo tipo `StudentName`, usando inicializadores de objeto. Este exemplo define as propriedades de `StudentName` tipo:
  
[!code-csharp[InitializerObjectExample](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/HowToObjectInitializers.cs#HowToObjectInitializers)]  

Os inicializadores de objeto podem ser usados para definir indexadores em um objeto. O exemplo a seguir define uma classe `BaseballTeam` que usa um indexador para obter e definir jogadores em posições diferentes. O inicializador pode atribuir jogadores com base na abreviação da posição ou no número usado para cada scorecard de beisebol de posição:

[!code-csharp[InitializerIndexerExample](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/HowToIndexInitializer.cs#HowToIndexInitializer)]  

## <a name="see-also"></a>Consulte também

- [Guia de Programação em C#](../index.md)
- [Inicializadores de objeto e coleção](object-and-collection-initializers.md)
