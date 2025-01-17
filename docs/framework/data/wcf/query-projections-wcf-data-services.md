---
title: Projeções de consulta (WCF Data Services)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- projection [WCF Data Services]
- WCF Data Services, projection
- query projection [WCF Data Services]
- WCF Data Services, querying
ms.assetid: a09f4985-9f0d-48c8-b183-83d67a3dfe5f
ms.openlocfilehash: 03fa40a895d322a8b5ad543f75424ef5b379672b
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74568896"
---
# <a name="query-projections-wcf-data-services"></a>Projeções de consulta (WCF Data Services)

A projeção fornece um mecanismo no Protocolo Open Data (OData) para reduzir a quantidade de dados no feed retornada por uma consulta, especificando que apenas determinadas propriedades de uma entidade são retornadas na resposta. Para obter mais informações, consulte [OData: selecionar opção de consulta do sistema ($SELECT)](https://go.microsoft.com/fwlink/?LinkId=186076).

Este tópico descreve como definir uma projeção de consulta, quais são os requisitos para tipos de entidade e não de entidade, fazer atualizações em resultados projetados, criar tipos projetados e listar algumas considerações de projeção.

## <a name="defining-a-query-projection"></a>Definindo uma projeção de consulta

Você pode adicionar uma cláusula de projeção a uma consulta usando a opção de consulta `$select` em um URI ou usando a cláusula [Select](../../../csharp/language-reference/keywords/select-clause.md) ([Select](../../../visual-basic/language-reference/queries/select-clause.md) na Visual Basic) em uma consulta LINQ. Os dados de entidade retornados podem ser projetados em tipos de entidade ou tipos que não são de entidade no cliente. Os exemplos neste tópico demonstram como usar a cláusula `select` em uma consulta LINQ.

> [!IMPORTANT]
> A perda de dados pode ocorrer no serviço de dados quando você salva atualizações que foram feitas em tipos projetados. Para obter mais informações, consulte [considerações de projeção](#considerations).

## <a name="requirements-for-entity-and-non-entity-types"></a>Requisitos para tipos de entidade e não de entidade

Os tipos de entidade devem ter uma ou mais propriedades de identidade que compõem a chave de entidade. Os tipos de entidade são definidos em clientes de uma das seguintes maneiras:

- Aplicando o <xref:System.Data.Services.Common.DataServiceKeyAttribute> ou <xref:System.Data.Services.Common.DataServiceEntityAttribute> ao tipo.

- Quando o tipo tem uma propriedade chamada `ID`.

- Quando o tipo tem uma propriedade chamada *type*`ID`, em que *Type* é o nome do tipo.

Por padrão, quando você projeta os resultados da consulta em um tipo definido no cliente, as propriedades solicitadas na projeção devem existir no tipo de cliente. No entanto, quando você especifica um valor de `true` para a propriedade <xref:System.Data.Services.Client.DataServiceContext.IgnoreMissingProperties%2A> da <xref:System.Data.Services.Client.DataServiceContext>, as propriedades especificadas na projeção não precisam ocorrer no tipo de cliente.

### <a name="making-updates-to-projected-results"></a>Fazendo atualizações nos resultados projetados

Quando você projeta resultados de consulta em tipos de entidade no cliente, o <xref:System.Data.Services.Client.DataServiceContext> pode rastrear esses objetos com atualizações a serem enviadas de volta ao serviço de dados quando o método de <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> é chamado. No entanto, as atualizações feitas aos dados projetadas em tipos que não são de entidade no cliente não podem ser enviadas de volta ao serviço de dados. Isso ocorre porque, sem uma chave para identificar a instância de entidade, o serviço de dados não pode atualizar a entidade correta na fonte de dados. Os tipos que não são de entidade não são anexados ao <xref:System.Data.Services.Client.DataServiceContext>.

Quando uma ou mais propriedades de um tipo de entidade definida no serviço de dados não ocorrem no tipo de cliente no qual a entidade está projetada, as inserções de novas entidades não conterá essas propriedades ausentes. Nesse caso, as atualizações feitas em entidades existentes **também** não incluirão essas propriedades ausentes. Quando existe um valor para tal propriedade, a atualização o redefine para o valor padrão da propriedade, conforme definido na fonte de dados.

### <a name="creating-projected-types"></a>Criando tipos projetados

O exemplo a seguir usa uma consulta LINQ anônima que projeta as propriedades relacionadas ao endereço do tipo `Customers` em um novo tipo de `CustomerAddress`, que é definido no cliente e é atribuído como um tipo de entidade:

[!code-csharp[Astoria Northwind Client#SelectCustomerAddressSpecific](~/samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#selectcustomeraddressspecific)]
[!code-vb[Astoria Northwind Client#SelectCustomerAddressSpecific](~/samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#selectcustomeraddressspecific)]

Neste exemplo, o padrão de inicializador de objeto é usado para criar uma nova instância do tipo `CustomerAddress` em vez de chamar um construtor. Não há suporte para construtores durante a projeção em tipos de entidade, mas eles podem ser usados durante a projeção em tipos anônimos e não de entidade. Como `CustomerAddress` é um tipo de entidade, as alterações podem ser feitas e enviadas de volta para o serviço de dados.

Além disso, os dados do tipo de `Customer` são projetados em uma instância do tipo de entidade `CustomerAddress` em vez de um tipo anônimo. Há suporte para projeção em tipos anônimos, mas os dados são somente leitura porque tipos anônimos são tratados como tipos que não são de entidade.

As configurações de <xref:System.Data.Services.Client.MergeOption> do <xref:System.Data.Services.Client.DataServiceContext> são usadas para resolução de identidade durante a projeção da consulta. Isso significa que, se uma instância do tipo de `Customer` já existir no <xref:System.Data.Services.Client.DataServiceContext>, uma instância de `CustomerAddress` com a mesma identidade seguirá as regras de resolução de identidade definidas pelo <xref:System.Data.Services.Client.MergeOption>

O seguinte descreve os comportamentos ao projetar resultados em tipos de entidade e não entidade:

**Criando uma nova instância projetada usando inicializadores**

- Exemplo:

   [!code-csharp[Astoria Northwind Client#ProjectWithInitializer](~/samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#projectwithinitializer)]
   [!code-vb[Astoria Northwind Client#ProjectWithInitializer](~/samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#projectwithinitializer)]

- Tipo de entidade: com suporte

- Tipo de não entidade: com suporte

**Criando uma nova instância projetada usando construtores**

- Exemplo:

   [!code-csharp[Astoria Northwind Client#ProjectWithConstructor](~/samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#projectwithconstructor)]
   [!code-vb[Astoria Northwind Client#ProjectWithConstructor](~/samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#projectwithconstructor)]

- Tipo de entidade: um <xref:System.NotSupportedException> é gerado.

- Tipo de não entidade: com suporte

**Usando projeção para transformar um valor de propriedade**

- Exemplo:

   [!code-csharp[Astoria Northwind Client#ProjectWithTransform](~/samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#projectwithtransform)]
   [!code-vb[Astoria Northwind Client#ProjectWithTransform](~/samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#projectwithtransform)]

- Tipo de entidade: essa transformação não tem suporte para tipos de entidade porque pode levar à confusão e potencialmente substituir os dados na fonte de dados que pertence a outra entidade. Um <xref:System.NotSupportedException> é gerado.

- Tipo de não entidade: com suporte

<a name="considerations"></a>

## <a name="projection-considerations"></a>Considerações sobre projeção

As considerações adicionais a seguir se aplicam ao definir uma projeção de consulta.

- Ao definir feeds personalizados para o formato Atom, você deve certificar-se de que todas as propriedades de entidade que têm mapeamentos personalizados definidos sejam incluídas na projeção. Quando uma propriedade de entidade mapeada não é incluída na projeção, pode ocorrer perda de dados. Para obter mais informações, consulte [personalização de feed](feed-customization-wcf-data-services.md).

- Quando são feitas inserções em um tipo projetado que não contém todas as propriedades da entidade no modelo de dados do serviço de dados, as propriedades não incluídas na projeção no cliente são definidas com seus valores padrão.

- Quando são feitas atualizações em um tipo projetado que não contém todas as propriedades da entidade no modelo de dados do serviço de dados, os valores existentes não incluídos na projeção no cliente serão substituídos por valores padrão não inicializados.

- Quando uma projeção inclui uma propriedade complexa, o objeto complexo inteiro deve ser retornado.

- Quando uma projeção inclui uma propriedade de navegação, os objetos relacionados são carregados implicitamente sem a necessidade de chamar o método <xref:System.Data.Services.Client.DataServiceQuery%601.Expand%2A>. Não há suporte para o método <xref:System.Data.Services.Client.DataServiceQuery%601.Expand%2A> para uso em uma consulta projetada.

- As consultas de projeções de consulta no cliente são convertidas para usar a opção de consulta `$select` no URI de solicitação. Quando uma consulta com projeção é executada em uma versão anterior do WCF Data Services que não oferece suporte à opção de consulta `$select`, um erro é retornado. Isso também pode acontecer quando a <xref:System.Data.Services.DataServiceBehavior.MaxProtocolVersion%2A> do <xref:System.Data.Services.DataServiceBehavior> do serviço de dados é definida com um valor de <xref:System.Data.Services.Common.DataServiceProtocolVersion.V1>. Para obter mais informações, consulte [controle de versão do serviço de dados](data-service-versioning-wcf-data-services.md).

Para obter mais informações, consulte [como: projetar resultados da consulta](how-to-project-query-results-wcf-data-services.md).

## <a name="see-also"></a>Consulte também

- [Querying the Data Service](querying-the-data-service-wcf-data-services.md) (Consultando o serviço de dados)
