---
title: LINQ to Objects (C#)
ms.date: 07/20/2015
ms.assetid: c5c2c178-3529-4f6c-b3df-2d5267af7f22
ms.openlocfilehash: b0cc47604b65a5883643d61b44b1e9878ec4b1bf
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2019
ms.locfileid: "74140883"
---
# <a name="linq-to-objects-c"></a>LINQ to Objects (C#)
O termo "LINQ to Objects" refere-se ao uso de consultas LINQ com qualquer coleção <xref:System.Collections.IEnumerable> ou <xref:System.Collections.Generic.IEnumerable%601> diretamente, sem o uso de uma API ou provedor LINQ intermediário como o [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md) ou [LINQ to XML](./linq-to-xml-overview.md). Você pode usar o LINQ para consultar qualquer coleção enumerável ​​como <xref:System.Collections.Generic.List%601>, <xref:System.Array> ou <xref:System.Collections.Generic.Dictionary%602>. A coleção pode ser definida pelo usuário ou pode ser devolvida por uma API do .NET Framework.  
  
 Basicamente, o LINQ to Objects representa uma nova abordagem às coleções. Na forma antiga, você precisava escrever loops `foreach` complexos que especificavam como recuperar dados de uma coleção. Na abordagem da LINQ, você escreve o código declarativo que descreve o que você deseja recuperar.  
  
 Além disso, as consultas LINQ oferecem três principais vantagens sobre os loops `foreach` tradicionais:  
  
1. Elas são mais concisas e legíveis, especialmente quando você filtra várias condições.  
  
2. Elas fornecem poderosos recursos de filtragem, ordenação e agrupamento com um mínimo de código do aplicativo.  
  
3. Elas podem ser movidas para outras fontes de dados com pouca ou nenhuma modificação.  
  
 Em geral, quanto mais complexa a operação que você deseja executar sobre os dados, maior benefício você perceberá usando consultas LINQs em vez de técnicas tradicionais de iteração.  
  
 O objetivo desta seção é demonstrar a abordagem LINQ com alguns exemplos selecionados. Não pretendemos que ela seja detalhada.  
  
## <a name="in-this-section"></a>Nesta seção  
 [LINQ e cadeias de caracteres (C#)](./linq-and-strings.md)  
 Explica como a LINQ pode ser usada para consultar e transformar cadeias de caracteres e coleções de cadeias de caracteres. Também inclui links para tópicos que demonstram esses princípios.  
  
 [LINQ e reflexão (C#)](how-to-query-an-assembly-s-metadata-with-reflection-linq.md)  
 Contém um link para um exemplo que demonstra como a LINQ usa a reflexão.  
  
 [LINQ e diretórios de arquivos (C#)](./linq-and-file-directories.md)  
 Explica como a LINQ pode ser usada para interagir com sistemas de arquivos. Também inclui links para tópicos que demonstram esses conceitos.  
  
 [Como consultar um ArrayList com LINQ (C#)](./how-to-query-an-arraylist-with-linq.md)  
 Demonstra como consultar um ArrayList no C#.  
  
 [Como adicionar métodos personalizados para consultas LINQ (C#)](./how-to-add-custom-methods-for-linq-queries.md)  
 Explica como estender o conjunto de métodos que você pode usar para consultas LINQ, adicionando os métodos de extensão à interface <xref:System.Collections.Generic.IEnumerable%601>.  
  
 [LINQ (consulta integrada à linguagem) (C#)](./index.md)  
 Fornece links para tópicos que explicam a LINQ e fornecem exemplos de código que realizam consultas.
