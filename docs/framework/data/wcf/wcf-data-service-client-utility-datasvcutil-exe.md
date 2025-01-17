---
title: Utilitário de cliente do WCF Data Service (DataSvcUtil.exe)
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, generating client data classes
- WCF Data Services, client library
- WCF Data Services, consuming
ms.assetid: 9d0af606-929b-4c03-b307-3ef5f705afce
ms.openlocfilehash: de260e1a6b58fdbac1a2f0f40c7ec2e50b13644e
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73975081"
---
# <a name="wcf-data-service-client-utility-datasvcutilexe"></a>Utilitário de cliente do WCF Data Service (DataSvcUtil.exe)

DataSvcUtil. exe é uma ferramenta de linha de comando fornecida por WCF Data Services que consome um feed Protocolo Open Data (OData) e gera as classes de serviço de dados do cliente que são necessárias para acessar um serviço de dados de um aplicativo cliente .NET Framework. Esse utilitário pode gerar classes de dados usando as seguintes fontes de metadados:

- O URI raiz de um serviço de dados. O utilitário solicita o documento de metadados de serviço, que descreve o modelo de dados exposto pelo serviço de dados. Para obter mais informações, consulte [OData: documento de metadados de serviço](https://go.microsoft.com/fwlink/?LinkId=186070).

- Um arquivo de modelo de dados (. CSDL) definido usando o CSDL (linguagem de definição de esquema conceitual), conforme definido no [\[MC-csdl\]: especificação de formato de arquivo de definição de esquema conceitual](https://go.microsoft.com/fwlink/?LinkID=159072) .

- Um arquivo. edmx criado usando as ferramentas de Modelo de Dados de Entidade fornecidas com o Entity Framework. Para obter mais informações, consulte o [\[MC-EDMX\]: modelo de dados de entidade para especificação de formato de empacotamento de serviços de dados](https://go.microsoft.com/fwlink/?LinkID=178833) .

Para obter mais informações, consulte [como: gerar manualmente classes de serviço de dados do cliente](how-to-manually-generate-client-data-service-classes-wcf-data-services.md).

A ferramenta DataSvcUtil. exe é instalada no diretório .NET Framework. Em muitos casos, isso está localizado em *C:\Windows\Microsoft.NET\Framework\v4.0*. Para sistemas de 64 bits, isso está localizado em *C:\Windows\Microsoft.NET\Framework64\v4.0*. Você também pode acessar a ferramenta DataSvcUtil. exe de Prompt de Comando do Desenvolvedor para Visual Studio.

## <a name="syntax"></a>Sintaxe

```console
datasvcutil /out:file [/in:file | /uri:serviceuri] [/dataservicecollection] [/language:devlang] [/nologo] [/version:ver] [/help]
```

## <a name="parameters"></a>Parâmetros

|Opção|Descrição|
|------------|-----------------|
|`/dataservicecollection`|Especifica que o código necessário para associar objetos a controles também é gerado.|
|`/help`<br /><br /> \- ou -<br /><br /> `/?`|Exibe sintaxe de comando e opções para a ferramenta.|
|*arquivo de\<`/in:` >*|Especifica o arquivo. CSDL ou. edmx ou um diretório onde o arquivo está localizado.|
|`/language:`[VB&#124;Csharp]|Especifica a linguagem dos arquivos de código-fonte gerados. A linguagem volta automaticamente para C#.|
|`/nologo`|Suprime a notificação de direitos autorais da exibição.|
|*arquivo de\<`/out:` >*|Especifica o nome do arquivo de código-fonte que contém as classes de serviço de dados do cliente geradas.|
|`/uri:` *\<cadeia de caracteres >*|O URI do feed OData.|
|`/version:`[1,0&#124;2,0]|Especifica a versão mais aceita do OData. A versão é determinada com base no atributo `DataServiceVersion` do elemento DataService nos metadados do serviço de dados retornados. Para obter mais informações, consulte [controle de versão do serviço de dados](data-service-versioning-wcf-data-services.md). Ao especificar o parâmetro `/dataservicecollection`, você também deve especificar `/version:2.0` para habilitar a vinculação de dados.|

## <a name="see-also"></a>Consulte também

- [Generating the Data Service Client Library](generating-the-data-service-client-library-wcf-data-services.md) (Gerando a biblioteca de clientes do serviço de dados)
- [Como adicionar uma referência de serviço de dados](how-to-add-a-data-service-reference-wcf-data-services.md)
