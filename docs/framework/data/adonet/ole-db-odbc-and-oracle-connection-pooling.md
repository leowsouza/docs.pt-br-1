---
title: OLE DB, ODBC e pool de conexões Oracle
ms.date: 03/30/2017
ms.assetid: 2bd83b1e-3ea9-43c4-bade-d9cdb9bbbb04
ms.openlocfilehash: 4b801032e67d1c4c51fed8556ff1fea05c214aff
ms.sourcegitcommit: ad800f019ac976cb669e635fb0ea49db740e6890
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73039841"
---
# <a name="ole-db-odbc-and-oracle-connection-pooling"></a>OLE DB, ODBC e pool de conexões Oracle
O pooling de conexões pode melhorar significativamente o desempenho e a escalabilidade do aplicativo. Esta seção discute o pooling de conexões para provedores de dados .NET Framework para OLE DB, ODBC e Oracle.  
  
## <a name="connection-pooling-for-oledb"></a>Pooling de conexões para OleDb  
 O Provedor de Dados do .NET Framework para OLE DB agrupa automaticamente conexões usando o pooling de sessão do OLE DB. Argumentos de cadeia de conexão podem ser usados para habilitar ou desabilitar os serviços do OLE DB que incluem pooling. Por exemplo, a cadeia de conexão a seguir desabilita o pooling de sessão do OLE DB e a inscrição automática de transação.  
  
```csharp
Provider=SQLOLEDB;OLE DB Services=-4;Data Source=localhost;Integrated Security=SSPI;  
```  
  
 É recomendável sempre fechar ou encerrar uma conexão quando você terminar de usá-la para retornar a conexão ao pool. As conexões que não são fechadas explicitamente não podem ser retornadas ao pool. Por exemplo, uma conexão que sai de escopo, mas que não foi fechada explicitamente será retornada somente para o pool de conexões se o tamanho do máximo tiver sido atingido e a conexão ainda estiver válida.  
  
 Para obter mais informações sobre OLE DB pooling de sessão ou de recursos, além de como desabilitar o pooling substituindo os padrões de serviço do provedor de OLE DB, consulte o [Guia do programador do OLE DB](https://go.microsoft.com/fwlink/?linkid=45232).  
  
## <a name="connection-pooling-for-odbc"></a>Pooling de conexões para Odbc  
 O pooling de conexões para o provedor de dados .NET Framework para ODBC é gerenciado pelo ODBC Driver Manager que é usado para a conexão, e não é afetado pelo provedor de dados .NET Framework para ODBC.  
  
 Para habilitar ou desabilitar o pool de conexões, abra o **administrador de fonte de dados ODBC** na pasta Ferramentas administrativas do painel de controle. A guia **pooling de conexão** permite especificar parâmetros de pool de conexões para cada driver ODBC instalado. Observe que as alterações do pooling de conexões para um driver ODBC específico afetam todos os aplicativos que usam o driver ODBC.  
  
## <a name="connection-pooling-for-oracleclient"></a>Pooling de conexões para OracleClient  
 O provedor de dados .NET Framework para Oracle fornece o pooling de conexões automaticamente para o aplicativo cliente do ADO.NET. Você também pode fornecer vários modificadores de cadeia de conexão para controlar o comportamento do pooling de conexões (consulte "Controlando o pooling de conexões com palavras-chave de cadeia de conexão", posteriormente neste tópico).  
  
### <a name="pool-creation-and-assignment"></a>Criação e atribuição de pools  
 Quando uma conexão é aberta, um pool de conexões é criado com base em um algoritmo compatível exato que associa o pool à cadeia de conexão na conexão. Cada pool de conexões é associado a uma cadeia de conexão distinta. Se a cadeia de conexão não for uma correspondência exata de um pool existente, quando uma nova conexão for aberta, um novo pool será criado.  
  
 Quando são criados, os pools de conexão não são destruídos até que o processo ativo termine. Manter pools inativos ou vazios usa muito poucos recursos do sistema.  
  
### <a name="connection-addition"></a>Adição de conexão  
 Um pool de conexões é criado para cada cadeia de conexão exclusiva. Quando um pool é criado, vários objetos de conexão são criados e adicionados ao pool para que o requisito de tamanho mínimo de pool seja atendido. As conexões são adicionadas ao pool quando necessário, até o tamanho máximo de pool.  
  
 Quando um objeto <xref:System.Data.OracleClient.OracleConnection> é solicitado, ele é obtido do pool caso haja uma conexão útil disponível. Para ser útil, a conexão não deve estar em uso no momento, deve ter um contexto de transação correspondente (ou não deve estar associada a nenhum contexto de transação) e deve ter um vínculo válido com o servidor.  
  
 Se o tamanho máximo de pool for atingido e nenhuma conexão útil estiver disponível, a solicitação será colocada na fila. O pool de conexões atende a essas solicitações realocando-as à medida que são retornadas ao pool. As conexões são retornadas para o pool quando fechadas ou descartadas.  
  
### <a name="connection-removal"></a>Remoção de conexão  
 O pooler de conexões remove uma conexão do pool depois que ela estiver ociosa por um período de tempo estendido ou caso detecte que a conexão com o servidor foi interrompida. Observe que isso pode ser detectado somente após uma tentativa de comunicação com o servidor. Se for encontrada uma conexão que não esteja mais conectada ao servidor, ela será marcada como inválida. O pooler de conexão verifica periodicamente os pools de conexão que procuram os objetos que foram lançados para o pool e que estão marcados como inválidos. Essas conexões são então removidas permanentemente.  
  
 Se uma conexão com um servidor desapareceu, ela pode ser removida do pool se o pooler de conexões não tiver detectado a conexão interrompida e a marcado como inválida. Quando isso ocorrer, uma exceção será gerada. No entanto, você ainda deverá fechar a conexão para liberá-la de volta para o pool.  
  
 Não chame `Close` nem `Dispose` em um objeto `Connection`, em um `DataReader` nem em nenhum outro objeto gerenciado no método `Finalize` de sua classe. Em um finalizador, libere somente recursos não gerenciados que sua classe possui diretamente. Se a classe não tiver nenhum recurso não gerenciado, não inclua um método `Finalize` em sua definição de classe. Para obter mais informações, consulte [coleta de lixo](../../../standard/garbage-collection/index.md).  
  
### <a name="transaction-support"></a>Suporte a transações  
 As conexões são removidas do pool e atribuídas com base no contexto de transação. O contexto do thread de solicitação e da conexão atribuída devem coincidir. Portanto, cada pool de conexões é, na verdade, subdividido em conexões sem nenhum contexto de transação associado a eles e em *N* subdivisãos que contêm conexões com um contexto de transação específico.  
  
 Quando uma conexão for fechada, ela será retornada ao pool e à subdivisão apropriada com base no contexto de transação. Portanto, é possível fechar a conexão sem gerar erros, mesmo que uma transação distribuída ainda esteja pendente. Isso permite confirmar ou anular a transação distribuída posteriormente.  
  
### <a name="controlling-connection-pooling-with-connection-string-keywords"></a>Controlando o pool de conexões com palavras-chave de cadeias de conexão  
 A propriedade <xref:System.Data.OracleClient.OracleConnection.ConnectionString%2A> do objeto <xref:System.Data.OracleClient.OracleConnection> dá suporte a pares chave-valor de cadeias de conexão que podem ser usados para ajustar o comportamento da lógica do pool de conexões.  
  
 A tabela a seguir descreve os valores de <xref:System.Data.OracleClient.OracleConnection.ConnectionString%2A> que você pode usar para ajustar o comportamento do pooling de conexões.  
  
|Name|Padrão|Descrição|  
|----------|-------------|-----------------|  
|`Connection Lifetime`|0|Quando uma conexão é retornada para o pool, seu tempo de criação é comparado com a hora atual e a conexão será destruída se esse intervalo de tempo (em segundos) exceder o valor especificado por `Connection Lifetime`. Isso é útil nas configurações clusterizadas para forçar o balanceamento de carga entre um servidor em execução e um servidor que acabou de ficar online.<br /><br /> Um valor de 0 (zero) fará as conexões com pool excederem o tempo limite máximo.|  
|`Enlist`|'true'|Quando for `true`, o pooler automaticamente inserirá a conexão no contexto de transação atual do thread de criação se um contexto de transação existir.|  
|`Max Pool Size`|100|O número máximo de conexões permitidas no pool.|  
|`Min Pool Size`|0|O número mínimo de conexões mantidas no pool.|  
|`Pooling`|'true'|Quando for `true`, o objeto de conexão será obtido do pool apropriado ou, se necessário, será criado e adicionado ao pool apropriado.|  
  
## <a name="see-also"></a>Consulte também

- [Pooling de Conexão](connection-pooling.md)
- [Contadores de desempenho](performance-counters.md)
- [ADO.NET Overview](ado-net-overview.md) (Visão geral do ADO.NET)
