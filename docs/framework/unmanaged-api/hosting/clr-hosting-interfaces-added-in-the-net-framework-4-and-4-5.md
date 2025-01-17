---
title: Interfaces de hospedagem CLR adicionadas no .NET Framework 4 e 4.5
ms.date: 03/30/2017
helpviewer_keywords:
- hosting interfaces [.NET Framework], version 4
- .NET Framework 4, hosting interfaces
- interfaces [.NET Framework hosting], version 4
ms.assetid: f6af6116-f5b0-4bda-a276-fffdba70893d
ms.openlocfilehash: aea88430d8f83234a1568bcaf433c2a75492e23a
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73195922"
---
# <a name="clr-hosting-interfaces-added-in-the-net-framework-4-and-45"></a>Interfaces de hospedagem CLR adicionadas no .NET Framework 4 e 4.5
Esta seção descreve as interfaces que os hosts não gerenciados podem usar para integrar o Common Language Runtime (CLR) no .NET Framework 4, .NET Framework 4,5 e versões posteriores em seus aplicativos. Essas interfaces fornecem métodos para um host configurar e carregar o tempo de execução em um processo.  
  
 A partir do .NET Framework 4, todas as interfaces de hospedagem têm as seguintes características:  
  
- Eles usam o gerenciamento de tempo de vida (`AddRef` e `Release`), encapsulamento (contexto implícito) e `QueryInterface` de COM.  
  
- Eles não usam tipos COM, como `BSTR`, `SAFEARRAY`ou `VARIANT`.  
  
- Não há modelos de apartamento, agregação ou ativação de registro que usam a [função CoCreateInstance](https://go.microsoft.com/fwlink/?LinkId=142894).  
  
## <a name="in-this-section"></a>Nesta seção  
 [Interface ICLRAppDomainResourceMonitor](../../../../docs/framework/unmanaged-api/hosting/iclrappdomainresourcemonitor-interface.md)  
 Fornece métodos que inspecionam o uso de memória e CPU de um domínio de aplicativo.  
  
 [Interface ICLRDomainManager](../../../../docs/framework/unmanaged-api/hosting/iclrdomainmanager-interface.md)  
 Permite que o host especifique o Gerenciador de domínio de aplicativo que será usado para inicializar o domínio de aplicativo padrão e para especificar as propriedades de inicialização.  
  
 [Interface ICLRGCManager2](../../../../docs/framework/unmanaged-api/hosting/iclrgcmanager2-interface.md)  
 Fornece o método [SetGCStartupLimitsEx](../../../../docs/framework/unmanaged-api/hosting/iclrgcmanager2-setgcstartuplimitsex-method.md) , que permite que um host defina o tamanho do segmento de coleta de lixo e o tamanho máximo da geração 0 do sistema de coleta de lixo com valores maiores que `DWORD`.  
  
 [Interface ICLRMetaHost](../../../../docs/framework/unmanaged-api/hosting/iclrmetahost-interface.md)  
 Fornece métodos que retornam uma versão específica do CLR, listam todos os CLRs instalados, listam todos os tempos de execução em processo, retornam a interface de ativação e detectam a versão do CLR usada para compilar um assembly.  
  
 [Interface ICLRMetaHostPolicy](../../../../docs/framework/unmanaged-api/hosting/iclrmetahostpolicy-interface.md)  
 Fornece o método [GetRequestedRuntime](../../../../docs/framework/unmanaged-api/hosting/iclrmetahostpolicy-getrequestedruntime-method.md) que fornece uma interface CLR baseada em critérios de política, assembly gerenciado, versão e arquivo de configuração.  
  
 [Interface ICLRRuntimeInfo](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)  
 Fornece métodos que retornam informações sobre um tempo de execução específico, incluindo versão, diretório e status de carregamento.  
  
 [Interface ICLRStrongName](../../../../docs/framework/unmanaged-api/hosting/iclrstrongname-interface.md)  
 Fornece funções estáticas globais básicas para assinar assemblies com nomes fortes. Todos os métodos [ICLRStrongName](../../../../docs/framework/unmanaged-api/hosting/iclrstrongname-interface.md) retornam o padrão com HResults.  
  
 [Interface ICLRStrongName2](../../../../docs/framework/unmanaged-api/hosting/iclrstrongname2-interface.md)  
 Fornece a capacidade de criar nomes fortes usando o grupo SHA-2 de algoritmos de hash seguro (SHA-256, SHA-384 e SHA-512).  
  
 [Interface ICLRTask2](../../../../docs/framework/unmanaged-api/hosting/iclrtask2-interface.md)  
 Fornece toda a funcionalidade da [interface ICLRTask](../../../../docs/framework/unmanaged-api/hosting/iclrtask-interface.md); Além disso, o fornece métodos que permitem que as anulações de thread sejam atrasadas no thread atual.  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Interfaces e coclasses de hospedagem CLR preteridas](../../../../docs/framework/unmanaged-api/hosting/deprecated-clr-hosting-interfaces-and-coclasses.md)  
 Descreve as interfaces de hospedagem fornecidas com as versões 1,0 e 1,1 do .NET Framework.  
  
 [Interfaces de hospedagem CLR](../../../../docs/framework/unmanaged-api/hosting/clr-hosting-interfaces.md)  
 Descreve as interfaces de hospedagem fornecidas com as .NET Framework versões 2,0, 3,0 e 3,5.  
  
 [Hospedagem](../../../../docs/framework/unmanaged-api/hosting/index.md)  
 Apresenta a hospedagem no .NET Framework.
