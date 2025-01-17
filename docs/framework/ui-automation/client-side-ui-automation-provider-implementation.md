---
title: Implementação de Provedor de Automação de Interface de Usuário do Lado do Cliente
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, client-side provider implementation
- client-side UI Automation provider, implementation
- provider implementation, UI Automation
ms.assetid: 3584c0a1-9cd0-4968-8b63-b06390890ef6
ms.openlocfilehash: 03df282022c39673a7e160dd5d79bdadd0c7adda
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74433995"
---
# <a name="client-side-ui-automation-provider-implementation"></a>Implementação de Provedor de Automação de Interface de Usuário do Lado do Cliente
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Várias estruturas de [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] diferentes estão em uso dentro dos sistemas operacionais da Microsoft, incluindo [!INCLUDE[TLA2#tla_win32](../../../includes/tla2sharptla-win32-md.md)], [!INCLUDE[TLA#tla_winforms](../../../includes/tlasharptla-winforms-md.md)]e [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)]. [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] expõe informações sobre elementos de interface do usuário aos clientes. No entanto, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] não tem conhecimento dos diferentes tipos de controles existentes nessas estruturas e nas técnicas necessárias para extrair informações delas. Em vez disso, ele deixa essa tarefa para objetos chamados provedores. Um provedor extrai informações de um controle específico e passa essas informações para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], que, em seguida, as apresenta ao cliente de maneira consistente.  
  
 Os provedores podem existir no lado do servidor ou no lado do cliente. Um provedor do lado do servidor é implementado pelo próprio controle. os elementos de [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] implementam provedores, assim como qualquer outro controle de terceiros escrito com [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] em mente.  
  
 No entanto, controles mais antigos como aqueles em [!INCLUDE[TLA2#tla_win32](../../../includes/tla2sharptla-win32-md.md)] e [!INCLUDE[TLA#tla_winforms](../../../includes/tlasharptla-winforms-md.md)] não dão suporte diretamente a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]. Esses controles são servidos em vez de provedores que existem no processo do cliente e obtêm informações sobre controles usando a comunicação entre processos; por exemplo, monitorando mensagens do Windows de e para os controles. Esses provedores do lado do cliente são chamados também de proxies.  
  
 O Windows Vista fornece provedores para controles padrão de [!INCLUDE[TLA2#tla_win32](../../../includes/tla2sharptla-win32-md.md)] e de Windows Forms. Além disso, um provedor de fallback fornece suporte parcial a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] para qualquer controle que não seja atendido por outro provedor ou proxy do lado do servidor, mas tenha uma implementação de Acessibilidade Ativa da Microsoft. Todos esses provedores são carregados e disponibilizados automaticamente para aplicativos cliente.  
  
 Para obter mais informações sobre o suporte para controles [!INCLUDE[TLA2#tla_win32](../../../includes/tla2sharptla-win32-md.md)] e Windows Forms, consulte [suporte à automação da interface do usuário para controles padrão](ui-automation-support-for-standard-controls.md).  
  
 Os aplicativos também podem registrar outros provedores do lado do cliente.  
  
<a name="Distributing_Client-Side_Providers"></a>   
## <a name="distributing-client-side-providers"></a>Distribuindo provedores do lado do cliente  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] espera encontrar provedores do lado do cliente em um assembly de código gerenciado. O namespace neste assembly deve ter o mesmo nome que o assembly. Por exemplo, um assembly chamado ContosoProxies. dll conterá o namespace ContosoProxies. No namespace, crie uma classe de <xref:UIAutomationClientsideProviders.UIAutomationClientSideProviders>. Na implementação do campo <xref:UIAutomationClientsideProviders.UIAutomationClientSideProviders.ClientSideProviderDescriptionTable> estático, crie uma matriz de estruturas <xref:System.Windows.Automation.ClientSideProviderDescription> descrevendo os provedores.  
  
<a name="Registering_and_Configuring_Client-Side_Providers"></a>   
## <a name="registering-and-configuring-client-side-providers"></a>Registrando e configurando provedores do lado do cliente  
 Os provedores do lado do cliente em uma DLL (biblioteca de vínculo dinâmico) são carregados chamando <xref:System.Windows.Automation.ClientSettings.RegisterClientSideProviderAssembly%2A>. Nenhuma ação adicional é exigida por um aplicativo cliente para fazer uso dos provedores.  
  
 Os provedores implementados no código próprio do cliente são registrados usando <xref:System.Windows.Automation.ClientSettings.RegisterClientSideProviders%2A>. Esse método usa como um argumento uma matriz de estruturas <xref:System.Windows.Automation.ClientSideProviderDescription>, cada uma delas especifica as seguintes propriedades:  
  
- Uma função de retorno de chamada que cria o objeto do provedor.  
  
- O nome da classe dos controles que o provedor vai fornecer.  
  
- O nome da imagem do aplicativo (geralmente o nome completo do arquivo executável) que o provedor irá fornecer.  
  
- Sinalizadores que regem como o nome da classe é correspondido em relação às classes de janela encontradas no aplicativo de destino.  
  
 Os dois últimos parâmetros são opcionais. O cliente pode especificar o nome da imagem do aplicativo de destino quando quiser usar provedores diferentes para aplicativos diferentes. Por exemplo, o cliente pode usar um provedor para um controle de exibição de lista [!INCLUDE[TLA2#tla_win32](../../../includes/tla2sharptla-win32-md.md)] em um aplicativo conhecido que dá suporte ao padrão de várias exibições e outro para um controle semelhante em outro aplicativo conhecido que não faz isso.  
  
## <a name="see-also"></a>Consulte também

- [Criar um provedor de automação de interface do usuário do lado do cliente](create-a-client-side-ui-automation-provider.md)
- [Implementar provedores de automação de interface do usuário em um aplicativo cliente](implement-ui-automation-providers-in-a-client-application.md)
