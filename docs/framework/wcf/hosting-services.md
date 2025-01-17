---
title: Serviços de hospedagem
ms.date: 03/30/2017
helpviewer_keywords:
- hosting services [WCF]
ms.assetid: 192be927-6be2-4fda-98f0-e513c4881acc
ms.openlocfilehash: 634e34eceeb3d3b8828a6d5ed85b6194bcf8586c
ms.sourcegitcommit: 9b2ef64c4fc10a4a10f28a223d60d17d7d249ee8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2019
ms.locfileid: "72961145"
---
# <a name="hosting-services"></a>Serviços de hospedagem

Para se tornar ativo, um serviço deve ser hospedado em um ambiente de tempo de execução que o cria e controla seu contexto e vida útil. Os serviços de Windows Communication Foundation (WCF) são projetados para serem executados em qualquer processo do Windows que ofereça suporte a código gerenciado.

O WCF fornece um modelo de programação unificado para a criação de aplicativos orientados a serviços. Esse modelo de programação permanece consistente e é independente do ambiente de tempo de execução no qual o serviço é implantado. Na prática, isso significa que o código para seus serviços parece ser muito parecido com a opção de hospedagem.

Essas opções de hospedagem vão desde a execução dentro de um aplicativo de console até ambientes de servidor, como um serviço do Windows em execução em um processo de trabalho gerenciado pelo Serviços de Informações da Internet (IIS) ou pelo WAS (serviço de ativação de processos do Windows). Os desenvolvedores escolhem o ambiente de hospedagem que cumpre os requisitos de implantação do serviço. Esses requisitos podem derivar da plataforma na qual o aplicativo é implantado, o transporte no qual ele deve enviar e receber mensagens ou o tipo de reciclagem de processo e outro gerenciamento de processo necessário para garantir a disponibilidade adequada, ou em alguns outros requisitos de gerenciamento ou de confiabilidade. A próxima seção fornece informações e orientações sobre opções de hospedagem.

## <a name="hosting-options"></a>Opções de hospedagem

### <a name="self-host-in-a-managed-application"></a>Auto-hospedar em um aplicativo gerenciado
 Os serviços WCF podem ser hospedados em qualquer aplicativo gerenciado. Essa é a opção mais flexível porque requer a menor infraestrutura para implantar. Você insere o código para o serviço dentro do código do aplicativo gerenciado e, em seguida, cria e abre uma instância do <xref:System.ServiceModel.ServiceHost> para disponibilizar o serviço. Para obter mais informações, consulte [como hospedar um serviço WCF em um aplicativo gerenciado](how-to-host-a-wcf-service-in-a-managed-application.md).

 Essa opção habilita dois cenários comuns: serviços WCF em execução dentro de aplicativos de console e aplicativos cliente avançados, como aqueles baseados em Windows Presentation Foundation (WPF) ou Windows Forms (WinForms). Hospedar um serviço WCF dentro de um aplicativo de console é normalmente útil durante a fase de desenvolvimento do aplicativo. Isso facilita a depuração, fácil de obter informações de rastreamento do para descobrir o que está acontecendo dentro do aplicativo e é fácil de mover ao copiá-los para novos locais. Essa opção de hospedagem também torna mais fácil para aplicativos cliente avançados, como aplicativos do WPF e WinForms, para se comunicar com o mundo exterior. Por exemplo, um cliente de colaboração ponto a ponto que usa o WPF para sua interface do usuário e também hospeda um serviço WCF que permite que outros clientes se conectem a ele e compartilhem informações.

### <a name="managed-windows-services"></a>Serviços gerenciados do Windows
 Essa opção de hospedagem consiste em registrar o AppDomain (domínio de aplicativo) que hospeda um serviço WCF como um serviço gerenciado do Windows (anteriormente conhecido como serviço NT) para que o tempo de vida do processo do serviço seja controlado pelo SCM (Gerenciador de controle de serviço) para Serviços do Windows. Como a opção de hospedagem interna, esse tipo de ambiente de hospedagem requer que alguns códigos de hospedagem sejam gravados como parte do aplicativo. O serviço é implementado como um serviço do Windows e como um serviço WCF, fazendo com que ele herde da classe <xref:System.ServiceProcess.ServiceBase>, bem como de uma interface de contrato de serviço do WCF. Em seguida, o <xref:System.ServiceModel.ServiceHost> é criado e aberto em um método <xref:System.ServiceProcess.ServiceBase.OnStart%28System.String%5B%5D%29> substituído e fechado em um método <xref:System.ServiceProcess.ServiceBase.OnStop> substituído. Uma classe de instalador que herda de <xref:System.Configuration.Install.Installer> também deve ser implementada para permitir que o programa seja instalado como um serviço do Windows pela ferramenta InstallUtil. exe. Para obter mais informações, consulte [como hospedar um serviço WCF em um serviço gerenciado do Windows](./feature-details/how-to-host-a-wcf-service-in-a-managed-windows-service.md). O cenário habilitado pela opção de Hospedagem de serviço do Windows gerenciado é o de um serviço WCF de longa execução hospedado fora do IIS em um ambiente seguro que não é ativado por mensagem. O tempo de vida do serviço é controlado pelo sistema operacional. Essa opção de hospedagem está disponível em todas as versões do Windows.

### <a name="internet-information-services-iis"></a>Serviços de Informações da Internet (IIS)
 A opção de hospedagem do IIS é integrada ao ASP.NET e usa os recursos oferecidos por essas tecnologias, como reciclagem de processo, desligamento ocioso, monitoramento de integridade do processo e ativação baseada em mensagem. Nos sistemas operacionais [!INCLUDE[wxp](../../../includes/wxp-md.md)] e [!INCLUDE[ws2003](../../../includes/ws2003-md.md)], essa é a solução preferida para hospedar aplicativos de serviço Web que devem ser altamente disponíveis e altamente escalonáveis. O IIS também oferece a capacidade de gerenciamento integrada que os clientes esperam de um produto de servidor de classe empresarial. Essa opção de hospedando requer que o IIS esteja configurado corretamente, mas não requer que nenhum código de hospedagem seja escrito como parte do aplicativo. Para obter mais informações sobre como configurar a hospedagem do IIS para um serviço WCF, consulte [como hospedar um serviço WCF no IIS](./feature-details/how-to-host-a-wcf-service-in-iis.md).

 Observe que os serviços hospedados pelo IIS só podem usar o transporte HTTP. Sua implementação no IIS 5,1 introduziu algumas limitações no [!INCLUDE[wxp](../../../includes/wxp-md.md)]. A ativação baseada em mensagem fornecida para um serviço WCF pelo IIS 5,1 em [!INCLUDE[wxp](../../../includes/wxp-md.md)] bloqueia qualquer outro serviço WCF hospedado internamente no mesmo computador usando a porta 80 para se comunicar. Os serviços WCF podem ser executados no mesmo AppDomain/pool de aplicativos/processo de trabalho que outros aplicativos quando hospedados pelo IIS 6,0 em [!INCLUDE[ws2003](../../../includes/ws2003-md.md)]. Mas como o WCF e o IIS 6,0 usam a pilha HTTP do modo kernel (HTTP. sys), o IIS 6,0 pode compartilhar a porta 80 com outros serviços WCF hospedados automaticamente em execução no mesmo computador, diferente do IIS 5,1.

### <a name="windows-process-activation-service-was"></a>Serviço de Ativação de Processos do Windows (WAS)
 O WAS (serviço de ativação de processos do Windows) é o novo mecanismo de ativação de processos para o [!INCLUDE[lserver](../../../includes/lserver-md.md)] que também está disponível em [!INCLUDE[wv](../../../includes/wv-md.md)]. Ele retém o conhecido modelo de processo do IIS 6,0 (pools de aplicativos e ativação de processo com base em mensagens) e recursos de hospedagem (como proteção rápida de falhas, monitoramento de integridade e reciclagem), mas remove a dependência de HTTP da ativação arquitectura. O IIS 7,0 usa o WAS para realizar a ativação baseada em mensagem via HTTP. Os componentes adicionais do WCF também se conectam ao WAS para fornecer ativação baseada em mensagem nos outros protocolos aos quais o WCF dá suporte, como TCP, MSMQ e pipes nomeados. Isso permite que os aplicativos que usam protocolos de comunicação usem os recursos do IIS, como reciclagem de processo, proteção rápida contra falhas e o sistema de configuração comum que só estavam disponíveis para aplicativos baseados em HTTP.

 Essa opção de hospedagem requer que o tenha sido configurado corretamente, mas não exige que você grave nenhum código de hospedagem como parte do aplicativo. Para obter mais informações sobre como configurar a hospedagem do WAS, consulte [como hospedar um serviço WCF no was](./feature-details/how-to-host-a-wcf-service-in-was.md).

## <a name="choose-a-hosting-environment"></a>Escolher um ambiente de hospedagem
 A tabela a seguir resume alguns dos principais benefícios e cenários associados a cada uma das opções de hospedagem.

|Ambiente de hospedagem|Cenários comuns|Principais benefícios e limitações|
|-------------------------|----------------------|----------------------------------|
|Aplicativo gerenciado ("auto-hospedado")|-Aplicativos de console usados durante o desenvolvimento.<br />-Aplicativos clientes WinForm e WPF ricos acessando serviços.|Flexível.<br />– Fácil de implantar.<br />-Não é uma solução empresarial para serviços.|
|Serviços do Windows (anteriormente conhecidos como serviços NT)|-Um serviço WCF de execução longa hospedado fora do IIS.|-Tempo de vida do processo de serviço controlado pelo sistema operacional, não ativado por mensagem.<br />-Com suporte em todas as versões do Windows.<br />-Ambiente seguro.|
|IIS 5,1, IIS 6,0|-Executando um serviço WCF lado a lado com conteúdo ASP.NET na Internet usando o protocolo HTTP.|-Reciclagem de processo.<br />-Desligamento ocioso.<br />-Monitoramento de integridade do processo.<br />-Ativação baseada em mensagem.<br />-Somente HTTP.|
|Serviço de Ativação de Processos do Windows (WAS)|-Executando um serviço WCF sem instalar o IIS na Internet usando vários protocolos de transporte.|-O IIS não é necessário.<br />-Reciclagem de processo.<br />-Desligamento ocioso.<br />-Monitoramento de integridade do processo.<br />-Ativação baseada em mensagem.<br />-Funciona com HTTP, TCP, pipes nomeados e MSMQ.|
|IIS 7.0|-Executando um serviço WCF com conteúdo ASP.NET.<br />-Executando um serviço WCF na Internet usando vários protocolos de transporte.|-FORAM os benefícios.<br />-Integrado ao conteúdo do ASP.NET e do IIS.|

 A escolha de um ambiente de hospedagem depende da versão do Windows na qual ele está implantado, dos transportes necessários para enviar mensagens e o tipo de processo e reciclagem de domínio de aplicativo necessários. A tabela a seguir resume os dados relacionados a esses requisitos.

|Ambiente de hospedagem|Disponibilidade da plataforma|Transportes com suporte|Reciclagem de processo e AppDomain|
|-------------------------|---------------------------|--------------------------|-------------------------------------|
|Aplicativos gerenciados ("auto-hospedado")|[!INCLUDE[wxp](../../../includes/wxp-md.md)], [!INCLUDE[ws2003](../../../includes/ws2003-md.md)], [!INCLUDE[wv](../../../includes/wv-md.md)],<br /><br /> [!INCLUDE[lserver](../../../includes/lserver-md.md)]|HTTP<br /><br /> NET. TCP,<br /><br /> NET. pipe,<br /><br /> NET. MSMQ|Não|
|Serviços do Windows (anteriormente conhecidos como serviços NT)|[!INCLUDE[wxp](../../../includes/wxp-md.md)], [!INCLUDE[ws2003](../../../includes/ws2003-md.md)], [!INCLUDE[wv](../../../includes/wv-md.md)],<br /><br /> [!INCLUDE[lserver](../../../includes/lserver-md.md)]|HTTP<br /><br /> NET. TCP,<br /><br /> NET. pipe,<br /><br /> NET. MSMQ|Não|
|IIS 5,1|[!INCLUDE[wxp](../../../includes/wxp-md.md)]|HTTP|Sim|
|IIS 6,0|[!INCLUDE[ws2003](../../../includes/ws2003-md.md)]|HTTP|Sim|
|Serviço de Ativação de Processos do Windows (WAS)|[!INCLUDE[wv](../../../includes/wv-md.md)], [!INCLUDE[lserver](../../../includes/lserver-md.md)]|HTTP<br /><br /> NET. TCP,<br /><br /> NET. pipe,<br /><br /> NET. MSMQ|Sim|

 É importante observar que a execução de um serviço ou de qualquer extensão de um host não confiável compromete a segurança. Além disso, observe que ao abrir um <xref:System.ServiceModel.ServiceHost> em representação, um aplicativo deve garantir que o usuário não esteja desconectado, por exemplo, armazenando em cache o <xref:System.Security.Principal.WindowsIdentity> do usuário.

## <a name="see-also"></a>Consulte também

- [Ciclo de vida de programação básica](basic-programming-lifecycle.md)
- [Implementando contratos de serviço](implementing-service-contracts.md)
- [Como hospedar um serviço WCF no IIS](./feature-details/how-to-host-a-wcf-service-in-iis.md)
- [Como hospedar um serviço do WCF no WAS](./feature-details/how-to-host-a-wcf-service-in-was.md)
- [Como hospedar um serviço WCF em um serviço Windows gerenciado](./feature-details/how-to-host-a-wcf-service-in-a-managed-windows-service.md)
- [Como hospedar um serviço do WCF em um aplicativo gerenciado](how-to-host-a-wcf-service-in-a-managed-application.md)
