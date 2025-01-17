---
title: Rota por corpo
ms.date: 03/30/2017
ms.assetid: 07a6fc3b-c360-42e0-b663-3d0f22cf4502
ms.openlocfilehash: dfe6d9e5a640efd9b516e0c0ff006ae0ed659834
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2019
ms.locfileid: "73424259"
---
# <a name="route-by-body"></a>Rota por corpo
Este exemplo demonstra como implementar um serviço que aceita objetos de mensagem com qualquer ação SOAP. Este exemplo é baseado no [introdução](../../../../docs/framework/wcf/samples/getting-started-sample.md) que implementa um serviço de calculadora. O serviço implementa uma única operação de `Calculate` que aceita um parâmetro de solicitação de <xref:System.ServiceModel.Channels.Message> e retorna uma resposta <xref:System.ServiceModel.Channels.Message>.  
  
 Neste exemplo, o cliente é um aplicativo de console (. exe) e o serviço é hospedado no IIS.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O exemplo demonstra a expedição de mensagens com base no conteúdo do corpo. O mecanismo interno de expedição de mensagens do modelo de serviço Windows Communication Foundation (WCF) baseia-se nas ações de mensagem. No entanto, há muitos serviços Web existentes que definem todas as suas operações com Action = "". É impossível criar um serviço baseado em WSDL que mantém a expedição de mensagens de solicitação com base nas informações de ação. Este exemplo demonstra um contrato de serviço baseado em WSDL (o WSDL está contido em Service. WSDL que é incluído com o exemplo). O contrato de serviço é calculadora, semelhante ao usado em [introdução](../../../../docs/framework/wcf/samples/getting-started-sample.md). No entanto, o `[OperationContract]` especifica `Action=""` para todas as operações.  
  
```csharp  
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples"),    
                 XmlSerializerFormat, DispatchByBodyBehavior]  
    public interface ICalculator  
    {  
        [OperationContract(Action="")]  
        double Add(double n1, double n2);  
        [OperationContract(Action = "")]  
        double Subtract(double n1, double n2);  
        [OperationContract(Action = "")]  
        double Multiply(double n1, double n2);  
        [OperationContract(Action = "")]  
        double Divide(double n1, double n2);  
    }  
```  
  
 Dado um contrato, um serviço requer o comportamento de expedição personalizado `DispatchByBodyBehavior` para permitir que as mensagens sejam expedidas entre as operações. Esse comportamento de expedição Inicializa o `DispatchByBodyElementOperationSelector` seletor de operação personalizada com uma tabela dos nomes de operação com chave por QName dos respectivos elementos de wrapper. `DispatchByBodyElementOperationSelector` examina a marca de início do primeiro filho do corpo e seleciona a operação usando a tabela mencionada anteriormente.  
  
 O cliente usa um proxy gerado automaticamente do WSDL exportado pelo serviço usando a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md).  
  
```console  
svcutil.exe  /n:http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples /uxs http://localhost/servicemodelsamples/service.svc?wsdl /out:generatedProxy.cs  
```  
  
 O fato de que as ações de todas as operações estão vazias não tem impacto no código do cliente, exceto nos parâmetros de ação no proxy gerado automaticamente.  
  
 O código do cliente executa vários cálculos. Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console
Add(100, 15.99) = 115.99  
Subtract(145, 76.54) = 68.46  
Multiply(9, 81.25) = 731.25  
Divide(22, 7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) para baixar todas as Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] amostras. Este exemplo está localizado no seguinte diretório.  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Interop\RouteByBody`  
