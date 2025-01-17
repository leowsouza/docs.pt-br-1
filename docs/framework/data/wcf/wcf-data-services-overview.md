---
title: Visão geral do WCF Data Services
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services
- WCF Data Services, about
ms.assetid: 7924cf94-c9a6-4015-afc9-f5d22b1743bb
ms.openlocfilehash: b3feb2a22fc38c6a2d13d6802b94386d6f7841ac
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74568798"
---
# <a name="wcf-data-services-overview"></a>Visão geral do WCF Data Services
WCF Data Services habilita a criação e o consumo de serviços de dados para a Web ou uma intranet usando o Protocolo Open Data (OData). O OData permite que você exponha seus dados como recursos que são endereçáveis por URIs. Isso permite que você acesse e altere dados usando a semântica da REST (transferência de estado de reapresentação), especificamente os verbos HTTP padrão de GET, PUT, POST e DELETE. Este tópico fornece uma visão geral dos padrões e das práticas definidas pelo OData e também dos recursos fornecidos pelo WCF Data Services para aproveitar o OData em aplicativos baseados em .NET Framework.  
  
## <a name="address-data-as-resources"></a>Dados de endereço como recursos  
 OData expõe dados como recursos endereçáveis por URIs. Os caminhos de recurso são construídos com base nas convenções de relacionamento de entidade do Modelo de Dados de Entidade. Nesse modelo, as entidades representam unidades operacionais de dados em um domínio de aplicativo, como clientes, pedidos, itens e produtos. Para obter mais informações, consulte [modelo de dados de entidade](../adonet/entity-data-model.md).  
  
 No OData, você endereça recursos de entidade como um conjunto de entidades que contém instâncias de tipos de entidade. Por exemplo, o URI <https://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/Orders> retorna todos os pedidos do serviço de dados `Northwind` que estão relacionados ao cliente com um valor `CustomerID` de `ALFKI.`  
  
 As expressões de consulta permitem executar operações tradicionais de consulta em recursos, como filtragem, classificação e paginação. Por exemplo, o URI <https://services.odata.org/Northwind/Northwind.svc/Customers( ' ALFKI ')/Orders? $filter = frete gt 50 > filtra os recursos para retornar apenas os pedidos com um custo de frete de mais de $50. Para obter mais informações, consulte [acessando recursos do serviço de dados](accessing-data-service-resources-wcf-data-services.md).  
  
## <a name="interoperable-data-access"></a>Acesso a dados interoperáveis  
 O OData se baseia em protocolos padrão de Internet para tornar os serviços de dados interoperáveis com aplicativos que não usam o .NET Framework. Como você pode usar URIs padrão para endereçar dados, seu aplicativo pode acessar e alterar os dados usando a semântica de REST (transferência de estado de reapresentação), especificamente os verbos HTTP padrão de GET, PUT, POST e DELETE. Isso permite acessar esses serviços de qualquer cliente que possa analisar e acessar dados transmitidos por protocolos HTTP padrão.  
  
 O OData define um conjunto de extensões para o protocolo de publicação Atom (AtomPub). Ele dá suporte a solicitações e respostas HTTP em mais de um formato de dados para acomodar vários aplicativos e plataformas cliente. Um feed OData pode representar dados em Atom, JavaScript Object Notation (JSON) e como XML simples. Quando Atom é o formato padrão, o formato do feed é especificado no cabeçalho da solicitação HTTP. Para obter mais informações, consulte formato [OData: Atom](https://go.microsoft.com/fwlink/?LinkID=185794) e [OData: JSON](https://go.microsoft.com/fwlink/?LinkID=185795).  
  
 Ao publicar dados como um feed OData, WCF Data Services se baseia em outros recursos da Internet existentes para operações como cache e autenticação. Para fazer isso, WCF Data Services integra-se com os aplicativos e serviços de hospedagem existentes, como ASP.NET, Windows Communication Foundation (WCF) e Serviços de Informações da Internet (IIS).  
  
## <a name="storage-independence"></a>Independência de armazenamento  
 Embora os recursos sejam resolvidos com base em um modelo de relacionamento de entidade, WCF Data Services expor feeds OData independentemente da fonte de dados subjacente. Depois que WCF Data Services aceita uma solicitação HTTP para um recurso que um URI identifica, a solicitação é desserializada e uma representação dessa solicitação é passada para um provedor de WCF Data Services. Esse provedor converte a solicitação em um formato específico da fonte de dados e executa a solicitação na fonte de dados subjacente. WCF Data Services Obtém a independência de armazenamento, separando o modelo conceitual que aborda os recursos prescritos pelo OData a partir do esquema específico da fonte de dados subjacente.  
  
 WCF Data Services integra-se com o Entity Framework ADO.NET para permitir que você crie serviços de dados que expõem dados relacionais. Você pode usar as ferramentas do Modelo de Dados de Entidade para criar um modelo de dados que contenha recursos endereçáveis como entidades e definir, ao mesmo tempo, o mapeamento entre esse modelo e as tabelas no banco de dados subjacente. Para obter mais informações, consulte [provedor de Entity Framework](entity-framework-provider-wcf-data-services.md).  
  
 WCF Data Services também permite que você crie serviços de dados que expõem todas as estruturas de dados que retornam uma implementação da interface <xref:System.Linq.IQueryable%601>. Isso permite criar serviços de dados que expõem dados de tipos .NET Framework. Há suporte para operações de criação, atualização e exclusão quando você também implementa a interface <xref:System.Data.Services.IUpdatable>. Para obter mais informações, consulte [provedor de reflexão](reflection-provider-wcf-data-services.md).  
  
 Para obter uma ilustração de como o WCF Data Services se integra a esses provedores de dados, consulte o diagrama arquitetônico mais adiante neste tópico.  
  
## <a name="custom-business-logic"></a>Lógica de negócios personalizada  
 WCF Data Services facilita a adição de lógica de negócios personalizada a um serviço de dados por meio de operações e interceptores de serviço. As operações de serviço são métodos definidos no servidor que são endereçáveis por URIs no mesmo formulário que os recursos de dados. As operações de serviço também podem usar a sintaxe de expressão de consulta para filtrar, ordenar e paginar dados retornados por uma operação. Por exemplo, o URI `http://localhost:12345/Northwind.svc/GetOrdersByCity?city='London'&$orderby=OrderDate&$top=10&$skip=10` representa uma chamada para uma operação de serviço chamada `GetOrdersByCity` no serviço de dados Northwind que retorna pedidos para clientes de Londres, com resultados paginados classificados por `OrderDate`. Para obter mais informações, consulte [operações de serviço](service-operations-wcf-data-services.md).  
  
 Os interceptores permitem que a lógica personalizada do aplicativo seja integrada ao processamento de mensagens de solicitação e de resposta por um serviço de dados. Os interceptores são chamados quando uma ação de consulta, inserção, atualização ou exclusão ocorre no conjunto de entidades especificado. Um interceptor poderá, então, alterar dados, impor uma política de autorização ou até mesmo terminar a operação. Os métodos de interceptor devem ser explicitamente registrados para um conjunto de entidades específico exposto por um serviço de dados. Para obter mais informações, consulte [Interceptors](interceptors-wcf-data-services.md).  
  
## <a name="client-libraries"></a>Bibliotecas de Cliente  
 O OData define um conjunto de padrões uniformes para interagir com os serviços de dados. Isso oferece uma oportunidade de criar componentes reutilizáveis baseados nesses serviços, como bibliotecas do lado do cliente que facilitam o consumo de serviços de dados.  
  
 WCF Data Services inclui bibliotecas de cliente para aplicativos cliente baseados em .NET Framework e Silverlight. Essas bibliotecas cliente permitem interagir com serviços de dados usando objetos .NET Framework. Eles também dão suporte a consultas baseadas em objeto e consultas LINQ, carregamento de objetos relacionados, controle de alterações e resolução de identidade. Para obter mais informações, consulte [WCF Data Services biblioteca de cliente](wcf-data-services-client-library.md).  
  
 Além das bibliotecas de cliente OData incluídas com o .NET Framework e com o Silverlight, há outras bibliotecas de cliente que permitem que você consuma um feed OData em aplicativos cliente, como PHP, AJAX e aplicativos Java. Para obter mais informações, consulte o [SDK do OData](https://go.microsoft.com/fwlink/?LinkID=185796).  
  
## <a name="architecture-overview"></a>Visão geral sobre arquitetura  
 O diagrama a seguir ilustra a arquitetura de WCF Data Services para expor feeds OData e usar esses feeds em bibliotecas de cliente habilitadas para OData:  
  
 ![Captura de tela mostrando um diagrama de arquitetura de WCF Data Services.](./media/wcf-data-services-overview/windows-communication-foundation-data-services-architecture.gif)  
  
## <a name="see-also"></a>Consulte também

- [WCF Data Services 4.5](index.md)
- [Introdução](getting-started-with-wcf-data-services.md)
- [Defining WCF Data Services](defining-wcf-data-services.md) (Definindo o WCF Data Services)
- [Acessando recursos do serviço de dados (WCF Data Services)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd728283(v=vs.100))
- [WCF Data Services Client Library](wcf-data-services-client-library.md) (Biblioteca de clientes do WCF Data Services)
- [Representational State Transfer (REST)](https://go.microsoft.com/fwlink/?LinkId=113919) [REST (Transferência de Estado Representacional)]
