---
title: Solucionando problemas de instalação
ms.date: 03/30/2017
ms.assetid: 1644f885-c408-4d5f-a5c7-a1a907bc8acd
ms.openlocfilehash: 586defea0f761f8b6dea691b778d221cff62c7cf
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74281603"
---
# <a name="troubleshooting-setup-issues"></a>Solucionando problemas de instalação
Este tópico descreve como solucionar problemas de configuração do Windows Communication Foundation (WCF).  
  
## <a name="some-windows-communication-foundation-registry-keys-are-not-repaired-by-performing-an-msi-repair-operation-on-the-net-framework-30"></a>Algumas chaves do Registro do Windows Communication Foundation não são reparadas executando uma operação de reparo do MSI no .NET Framework 3.0  
 Se você excluir algumas das seguintes chaves do Registro:  
  
- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ServiceModelService 3.0.0.0  
  
- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ServiceModelOperation 3.0.0.0  
  
- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ServiceModelEndpoint 3.0.0.0  
  
- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SMSvcHost 3.0.0.0  
  
- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSDTC Bridge 3.0.0.0  
  
 As chaves não serão recriadas se você executar reparar usando o instalador .NET Framework 3,0 iniciado no miniaplicativo **Adicionar/remover programas** no painel de **controle**. Para recriar corretamente essas chaves, o usuário deverá desinstalar e reinstalar o .NET Framework 3.0.  
  
## <a name="wmi-service-corruption-blocks-installation-of-the-windows-communication-foundation-wmi-provider-during-installation-of-net-framework-30-package"></a>O WMI Service Corruption bloqueia a instalação do provedor WMI do Windows Communication Foundation durante a instalação do pacote do .NET Framework 3.0  
 O WMI Service Corruption pode bloquear a instalação do provedor WMI do Windows Communication Foundation. Durante a instalação, o instalador do Windows Communication Foundation não é capaz de registrar o arquivo .mof do WCF usando o componente mofcomp.exe. Veja a seguir uma lista de sintomas:  
  
1. A instalação do .NET Framework 3.0 é concluída com sucesso, mas o provedor WMI do WCF não é registrado.  
  
2. Um evento de erro aparece no log de eventos do aplicativo que faz referência aos problemas registrando o provedor WMI para WCF ou executando mofcomp.exe.  
  
3. O arquivo de log de configuração chamado dd_wcf_retCA* no diretório %temp% do usuário contém referências a falhas para registrar o provedor WMI do WCF.  
  
4. Uma exceção como a seguinte pode ser listada no log de eventos ou no arquivo de log de rastreamento de configuração:  
  
     ServiceModelReg [11:09:59:046]: System.ApplicationException: Unexpected result 3 executing E:\WINDOWS\system32\wbem\mofcomp.exe with "E:\WINDOWS\Microsoft.NET\Framework\v3.0\Windows Communication Foundation\ServiceModel.mof"  
  
     ou:  
  
     ServiceModelReg [07:19:33:843]: System.TypeInitializationException: The type initializer for 'System.Management.ManagementPath' threw an exception. ---> System. Runtime. InteropServices. COMException (0x80040154): falha na recuperação da fábrica de classes COM para o componente com CLSID {CF4CC405-E2C5-4DDD-B3CE-5E7582D8C9FA} devido ao seguinte erro: 80040154.  
  
     ou:  
  
     ServiceModelReg [07:19:32:750]: System.IO.FileNotFoundException: Could not load file or assembly 'C:\WINDOWS\system32\wbem\mofcomp.exe' or one of its dependencies. O sistema não pode encontrar o arquivo especificado.  
  
     Nome de arquivo: 'C:\WINDOWS\system32\wbem\mofcomp.exe  
  
 As etapas a seguir devem ser seguidas para resolver o problema descrito anteriormente.  
  
1. Execute [o utilitário de diagnóstico WMI, versão 2,0,](https://go.microsoft.com/fwlink/?LinkId=94685) para reparar o serviço WMI. Para obter mais informações sobre como usar essa ferramenta, consulte o tópico [Utilitário de diagnóstico WMI](https://go.microsoft.com/fwlink/?LinkId=94686) .  
  
 Repare a instalação do .NET Framework 3,0 usando o miniaplicativo **Adicionar/remover programas** localizado no **painel de controle**ou desinstale/reinstale o .NET Framework 3,0.  
  
## <a name="repairing-net-framework-30-after-net-framework-35-installation-removes-configuration-elements-introduced-by-net-framework-35-in-machineconfig"></a>Reparar o .NET Framework 3.0 após a instalação do .NET Framework 3.5 remove os elementos de configuração introduzidos pelo .NET Framework 3.5 em machine.config  
 Se você fizer um reparo do .NET Framework 3,0 depois de instalar o .NET Framework 3,5, os elementos de configuração introduzidos pelo .NET Framework 3,5 em Machine. config serão removidos. No entanto, o web.config permanecerá intacto. A solução alternativa é reparar .NET Framework 3,5 após isso via ARP ou usar a [ferramenta de registro de serviço de fluxo de trabalho (WFServicesReg. exe)](workflow-service-registration-tool-wfservicesreg-exe.md) com a opção `/c`.  
  
 A [ferramenta de registro do serviço de fluxo de trabalho (WFServicesReg. exe)](workflow-service-registration-tool-wfservicesreg-exe.md) pode ser encontrada em%windir%\Microsoft.NET\framework\v3.5\ ou%windir%\Microsoft.NET\framework64\v3.5\  
  
## <a name="configure-iis-properly-for-wcfwf-webhost-after-installing-net-framework-35"></a>Configure o IIS corretamente para WCF/WF Webhost após instalar o .NET Framework 3.5  
 Quando .NET Framework instalação do 3,5 falha ao definir definições adicionais de configuração do IIS relacionadas ao WCF, ele registra um erro no log de instalação e continua. Qualquer tentativa de executar aplicativos do WorkflowServices falhará, porque os parâmetros de configuração necessários estão faltando. Por exemplo, carregar xoml ou o serviço de regras pode falhar.  
  
 Para solucionar esse problema, use a [ferramenta de registro do serviço de fluxo de trabalho (WFServicesReg. exe)](workflow-service-registration-tool-wfservicesreg-exe.md) com a opção `/c` para configurar corretamente os mapas de script do IIS no computador. A [ferramenta de registro do serviço de fluxo de trabalho (WFServicesReg. exe)](workflow-service-registration-tool-wfservicesreg-exe.md) pode ser encontrada em%windir%\Microsoft.NET\framework\v3.5\ ou%windir%\Microsoft.NET\framework64\v3.5\  
  
## <a name="could-not-load-type-systemservicemodelactivationhttpmodule-from-assembly-systemservicemodel-version-3000-cultureneutral-publickeytokenb77a5c561934e089"></a>Não foi possível carregar o tipo ‘System.ServiceModel.Activation.HttpModule’ do assembly ‘System.ServiceModel, Version 3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089’  
 Esse erro ocorrerá se .NET Framework 4 estiver instalado e a ativação de HTTP do WCF estiver habilitada. Para resolver o problema, execute a seguinte linha de comando de dentro do Prompt de Comando do Desenvolvedor para Visual Studio:  
  
```console
aspnet_regiis.exe -i -enable  
```  
  
## <a name="see-also"></a>Consulte também

- [Instruções de configuração](./samples/set-up-instructions.md)
