---
title: Agrupando dados (C#)
ms.date: 07/20/2015
ms.assetid: e414e9e4-343a-4e6e-858f-4a30c5e64492
ms.openlocfilehash: e7f10b121a7a1c599d88731a806fe784eb1a7e66
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2019
ms.locfileid: "73423413"
---
# <a name="grouping-data-c"></a>Agrupando dados (C#)
O agrupamento refere-se à operação de colocação de dados em grupos, de modo que os elementos em cada grupo compartilhem um atributo comum.  
  
 A ilustração a seguir mostra os resultados do agrupamento de uma sequência de caracteres. A chave para cada grupo é o caractere.  
  
 ![Diagrama que mostra uma operação de Agrupamento do LINQ.](./media/grouping-data/linq-group-operation.png)  
  
 Os métodos do operador de consulta padrão que agrupam elementos de dados estão listados na seção a seguir.  
  
## <a name="methods"></a>Métodos  
  
|Nome do método|Descrição|Sintaxe de expressão de consulta C#|Mais informações|  
|-----------------|-----------------|---------------------------------|----------------------|  
|GroupBy|Agrupa elementos que compartilham um atributo comum. Cada grupo é representado por um objeto <xref:System.Linq.IGrouping%602>.|`group … by`<br /><br /> \- ou -<br /><br /> `group … by … into …`|<xref:System.Linq.Enumerable.GroupBy%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.GroupBy%2A?displayProperty=nameWithType>|  
|ToLookup|Insere os elementos em um <xref:System.Linq.Lookup%602> (um dicionário one-to-many) com base em uma função de seletor de chave.|Não aplicável.|<xref:System.Linq.Enumerable.ToLookup%2A?displayProperty=nameWithType>|  
  
## <a name="query-expression-syntax-example"></a>Exemplo de sintaxe de expressão de consulta  
 O seguinte exemplo de código usa a cláusula `group by` para agrupar inteiros em uma lista de acordo com se eles são pares ou ímpares.  
  
```csharp  
List<int> numbers = new List<int>() { 35, 44, 200, 84, 3987, 4, 199, 329, 446, 208 };  
  
IEnumerable<IGrouping<int, int>> query = from number in numbers  
                                         group number by number % 2;  
  
foreach (var group in query)  
{  
    Console.WriteLine(group.Key == 0 ? "\nEven numbers:" : "\nOdd numbers:");  
    foreach (int i in group)  
        Console.WriteLine(i);  
}  
  
/* This code produces the following output:  
  
    Odd numbers:  
    35  
    3987  
    199  
    329  
  
    Even numbers:  
    44  
    200  
    84  
    4  
    446  
    208  
*/  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Linq>
- [Visão geral de operadores de consulta padrão (C#)](./standard-query-operators-overview.md)
- [Cláusula group](../../../language-reference/keywords/group-clause.md)
- [Como criar um grupo aninhado](../../../linq/create-a-nested-group.md)
- [Como agrupar arquivos por extensão (LINQ) (C#)](./how-to-group-files-by-extension-linq.md)
- [Como agrupar resultados de consultas](../../../linq/group-query-results.md)
- [Como executar uma subconsulta em uma operação de agrupamento](../../../linq/perform-a-subquery-on-a-grouping-operation.md)
- [Como dividir um arquivo em vários arquivos usando grupos (LINQ) (C#)](./how-to-split-a-file-into-many-files-by-using-groups-linq.md)
