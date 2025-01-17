---
title: Automação de Eventos de Interface de Usuário para Clientes.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, events for clients
- events, UI Automation clients
ms.assetid: b909e388-3f24-4997-b6d4-bd9c35c2dc27
ms.openlocfilehash: 4193f92223cb9c9f924c1021b2f3e58a5e8b988d
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74441511"
---
# <a name="ui-automation-events-for-clients"></a>Automação de Eventos de Interface de Usuário para Clientes.
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico descreve como [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] eventos são usados por clientes de automação da interface do usuário.  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] permite que os clientes assinem eventos de interesse. Esse recurso melhora o desempenho ao eliminar a necessidade de sondar continuamente todos os elementos de [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] no sistema para ver se alguma informação, estrutura ou estado foi alterada.  
  
 A eficiência também é aprimorada pela capacidade de escutar eventos somente dentro de um escopo definido. Por exemplo, um cliente pode escutar eventos de alteração de foco em todos os elementos de [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] na árvore, ou em apenas um elemento e seus descendentes.  
  
> [!NOTE]
> Não presuma que todos os eventos possíveis sejam gerados por um provedor de [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)]. Por exemplo, nem todas as alterações de propriedade fazem com que eventos sejam gerados pelos provedores de proxy padrão para controles [!INCLUDE[TLA#tla_winforms](../../../includes/tlasharptla-winforms-md.md)] e [!INCLUDE[TLA2#tla_win32](../../../includes/tla2sharptla-win32-md.md)].  
  
 Para obter uma visão mais ampla de eventos de [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [visão geral dos eventos de automação da interface do usuário](ui-automation-events-overview.md).  
  
<a name="Subscribing_to_Events"></a>   
## <a name="subscribing-to-events"></a>Assinando eventos  
 Os aplicativos cliente assinam eventos de um tipo específico registrando um manipulador de eventos, usando um dos métodos a seguir.  
  
|Método|Tipo de Evento|Tipo de argumentos de evento|Tipo de delegado|  
|------------|----------------|--------------------------|-------------------|  
|<xref:System.Windows.Automation.Automation.AddAutomationFocusChangedEventHandler%2A>|Alteração de foco|<xref:System.Windows.Automation.AutomationFocusChangedEventArgs>|<xref:System.Windows.Automation.AutomationFocusChangedEventHandler>|  
|<xref:System.Windows.Automation.Automation.AddAutomationPropertyChangedEventHandler%2A>|Alteração de propriedade|<xref:System.Windows.Automation.AutomationPropertyChangedEventArgs>|<xref:System.Windows.Automation.AutomationPropertyChangedEventHandler>|  
|<xref:System.Windows.Automation.Automation.AddStructureChangedEventHandler%2A>|Alteração de estrutura|<xref:System.Windows.Automation.StructureChangedEventArgs>|<xref:System.Windows.Automation.StructureChangedEventHandler>|  
|<xref:System.Windows.Automation.Automation.AddAutomationEventHandler%2A>|Todos os outros eventos, identificados por um <xref:System.Windows.Automation.AutomationEvent>|<xref:System.Windows.Automation.AutomationEventArgs> ou <xref:System.Windows.Automation.WindowClosedEventArgs>|<xref:System.Windows.Automation.AutomationEventHandler>|  
  
 Antes de chamar o método, você deve criar um método delegate para manipular o evento. Se preferir, você pode manipular diferentes tipos de eventos em um único método e passar esse método em várias chamadas para um dos métodos na tabela. Por exemplo, um único <xref:System.Windows.Automation.AutomationEventHandler> pode ser configurado para lidar com vários eventos de forma diferente, de acordo com o <xref:System.Windows.Automation.AutomationEventArgs.EventId%2A>.  
  
> [!NOTE]
> Para processar eventos fechados por janela, converta o tipo de argumento que é passado para o manipulador de eventos como <xref:System.Windows.Automation.WindowClosedEventArgs>. Como o elemento [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] da janela não é mais válido, você não pode usar o parâmetro `sender` para recuperar informações; em vez disso, use <xref:System.Windows.Automation.WindowClosedEventArgs.GetRuntimeId%2A>.  
  
> [!CAUTION]
> Se seu aplicativo puder receber eventos de sua própria [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)], não use o thread de [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] do seu aplicativo para assinar eventos ou cancelar a assinatura. Isso pode levar a um comportamento imprevisível. Para obter mais informações, consulte [problemas de threading de automação da interface do usuário](ui-automation-threading-issues.md).  
  
 No desligamento ou quando [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos não são mais interessantes para o aplicativo, os clientes de automação da interface do usuário devem chamar um dos métodos a seguir.  
  
|Método|Descrição|  
|------------|-----------------|  
|<xref:System.Windows.Automation.Automation.RemoveAutomationEventHandler%2A>|Cancela o registro de um manipulador de eventos que foi registrado usando <xref:System.Windows.Automation.Automation.AddAutomationEventHandler%2A>.|  
|<xref:System.Windows.Automation.Automation.RemoveAutomationFocusChangedEventHandler%2A>|Cancela o registro de um manipulador de eventos que foi registrado usando <xref:System.Windows.Automation.Automation.AddAutomationFocusChangedEventHandler%2A>.|  
|<xref:System.Windows.Automation.Automation.RemoveAutomationPropertyChangedEventHandler%2A>|Cancela o registro de um manipulador de eventos que foi registrado usando <xref:System.Windows.Automation.Automation.AddAutomationPropertyChangedEventHandler%2A>.|  
|<xref:System.Windows.Automation.Automation.RemoveAllEventHandlers%2A>|Cancela o registro de todos os manipuladores de eventos registrados.|  
  
 Para obter um exemplo de código, consulte [assinar eventos de automação da interface do usuário](subscribe-to-ui-automation-events.md).  
  
## <a name="see-also"></a>Consulte também

- [Assinar eventos de automação de interface do usuário](subscribe-to-ui-automation-events.md)
- [Visão geral sobre eventos de automação de interface do usuário](ui-automation-events-overview.md)
- [Visão geral de propriedades de automação de interface do usuário](ui-automation-properties-overview.md)
- [Exemplo de TrackFocus](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/FocusTracker)
