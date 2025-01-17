---
title: Campo genérico e métodos de SetField (LINQ to DataSet)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1883365f-9d6c-4ccb-9187-df309f47706d
ms.openlocfilehash: 1b2c7434543bb2574c59eaec126a621121dd7cef
ms.sourcegitcommit: b1cfd260928d464d91e20121f9bdba7611c94d71
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67504789"
---
# <a name="generic-field-and-setfield-methods-linq-to-dataset"></a>Campo genérico e métodos de SetField (LINQ to DataSet)
O LINQ to DataSet fornece métodos de extensão para o <xref:System.Data.DataRow> classe para acessar valores de coluna: o <xref:System.Data.DataRowExtensions.Field%2A> método e o <xref:System.Data.DataRowExtensions.SetField%2A> método. Esses métodos fornecem acesso fácil aos valores de coluna para os desenvolvedores, especialmente em relação aos valores nulos. O <xref:System.Data.DataSet> usa <xref:System.DBNull.Value?displayProperty=nameWithType> para representar valores nulos, enquanto [!INCLUDE[vbteclinq](../../../../includes/vbteclinq-md.md)] usa o <xref:System.Nullable> e <xref:System.Nullable%601> tipos. Usando o acessador pré-existente de coluna no <xref:System.Data.DataRow> exige que você converta o objeto de retorno para o tipo apropriado. Se um campo específico em uma <xref:System.Data.DataRow> pode ser nulo, você deve verificar explicitamente um valor nulo porque retornar <xref:System.DBNull.Value?displayProperty=nameWithType> e implicitamente convertê-lo para outro tipo gera um <xref:System.InvalidCastException>. No exemplo a seguir, se o <xref:System.Data.DataRow.IsNull%2A?displayProperty=nameWithType> método não foi usado para verificar se há um valor nulo, uma exceção seria lançada se o indexador retornado <xref:System.DBNull.Value?displayProperty=nameWithType> e tentou convertê-lo em um <xref:System.String>.  
  
 [!code-csharp[DP LINQ to DataSet Examples#WhereIsNull](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#whereisnull)]
 [!code-vb[DP LINQ to DataSet Examples#WhereIsNull](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#whereisnull)]  
  
 O método <xref:System.Data.DataRowExtensions.Field%2A> fornece acesso aos valores de coluna de um <xref:System.Data.DataRow> e o <xref:System.Data.DataRowExtensions.SetField%2A> define valores de coluna em um <xref:System.Data.DataRow>. Os métodos <xref:System.Data.DataRowExtensions.Field%2A> e <xref:System.Data.DataRowExtensions.SetField%2A> lidam com tipos anuláveis, para você não precise verificar valores nulos explicitamente como no exemplo anterior. Ambos os métodos também são genéricos, de modo que você não tem que converter o tipo de retorno.  
  
 O exemplo a seguir usa o método <xref:System.Data.DataRowExtensions.Field%2A>.  
  
 [!code-csharp[DP LINQ to DataSet Examples#Where3](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#where3)]
 [!code-vb[DP LINQ to DataSet Examples#Where3](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#where3)]  
  
 Observe que o tipo de dados especificado no parâmetro genérico `T` dos métodos <xref:System.Data.DataRowExtensions.Field%2A> e <xref:System.Data.DataRowExtensions.SetField%2A> deve corresponder ao tipo do valor subjacente. Caso contrário, uma exceção <xref:System.InvalidCastException> será gerada. O nome da coluna especificado também deve corresponder ao nome de uma coluna no <xref:System.Data.DataSet> ou <xref:System.ArgumentException> será gerado. Em ambos os casos, a exceção é gerada em tempo de execução durante a enumeração dos dados quando a consulta é executada.  
  
 O próprio método <xref:System.Data.DataRowExtensions.SetField%2A> não realiza nenhuma conversão de tipos. Isso não significa, entretanto, que uma conversão de tipos não ocorrerá. O <xref:System.Data.DataRowExtensions.SetField%2A> método expõe o comportamento ADO.NET do <xref:System.Data.DataRow> classe. Uma conversão de tipo de ação pode ser realizada a <xref:System.Data.DataRow> objeto e o valor convertido seriam salvo para o <xref:System.Data.DataRow> objeto.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Data.DataRowExtensions>
