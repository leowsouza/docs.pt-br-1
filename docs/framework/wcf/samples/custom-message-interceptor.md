---
title: Interceptor de mensagem personalizado
ms.date: 03/30/2017
ms.assetid: 73f20972-53f8-475a-8bfe-c133bfa225b0
ms.openlocfilehash: 61f9bae24f5edb70430f4f3eaa16e42da221a7b4
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73978306"
---
# <a name="custom-message-interceptor"></a>Interceptor de mensagem personalizado
Este exemplo demonstra o uso do modelo de extensibilidade do canal. Em particular, ele mostra como implementar um elemento de ligação personalizado que cria fábricas de canal e ouvintes de canal para interceptar todas as mensagens recebidas e enviadas em um ponto específico na pilha de tempo de execução. O exemplo também inclui um cliente e um servidor que demonstram o uso dessas fábricas personalizadas.  
  
 Neste exemplo, o cliente e o serviço são programas de console (. exe). O cliente e o serviço fazem uso de uma biblioteca comum (. dll) que contém o elemento de associação personalizado e seus objetos de tempo de execução associados.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) para baixar todas as Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] amostras. Este exemplo está localizado no seguinte diretório.  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Channels\MessageInterceptor`  
  
 O exemplo descreve o procedimento recomendado para criar um canal em camadas personalizado no Windows Communication Foundation (WCF), usando a estrutura de canal e as práticas recomendadas do WCF a seguir. As etapas para criar um canal personalizado em camadas são as seguintes:  
  
1. Decida em quais formas de canal o Channel Factory e o ouvinte de canal irão dar suporte.  
  
2. Crie uma fábrica de canais e um ouvinte de canal que ofereçam suporte às suas formas de canal.  
  
3. Adicione um elemento Binding que adiciona o canal em camadas personalizado a uma pilha de canais.  
  
4. Adicione uma seção de extensão de elemento de associação para expor o novo elemento de associação ao sistema de configuração.  
  
## <a name="channel-shapes"></a>Formas de canal  
 A primeira etapa na gravação de um canal personalizado em camadas é decidir quais formas são necessárias para o canal. Para nosso Inspetor de mensagem, damos suporte a qualquer forma com a qual a camada abaixo dá suporte (por exemplo, se a camada abaixo pode criar <xref:System.ServiceModel.Channels.IOutputChannel> e <xref:System.ServiceModel.Channels.IDuplexSessionChannel>, também expõemos <xref:System.ServiceModel.Channels.IOutputChannel> e <xref:System.ServiceModel.Channels.IDuplexSessionChannel>).  
  
## <a name="channel-factory-and-listener-factory"></a>Fábrica de canais e fábrica de ouvintes  
 A próxima etapa na gravação de um canal personalizado em camadas é criar uma implementação de <xref:System.ServiceModel.Channels.IChannelFactory> para canais de cliente e de <xref:System.ServiceModel.Channels.IChannelListener> para canais de serviço.  
  
 Essas classes usam uma fábrica interna e um ouvinte e delegam todas as chamadas `OnCreateChannel` e `OnAcceptChannel` para a fábrica interna e o ouvinte.  
  
```csharp
class InterceptingChannelFactory<TChannel> : ChannelFactoryBase<TChannel>  
{ 
    //... 
}

class InterceptingChannelListener<TChannel> : ListenerFactoryBase<TChannel>  
{ 
    //...
}  
```  
  
## <a name="adding-a-binding-element"></a>Adicionando um elemento de associação  
 O exemplo define um elemento de associação personalizado: `InterceptingBindingElement`. `InterceptingBindingElement` usa uma `ChannelMessageInterceptor` como uma entrada e usa essa `ChannelMessageInterceptor` para manipular mensagens que passam por ela. Essa é a única classe que deve ser pública. A fábrica, o ouvinte e os canais podem ser implementações internas das interfaces de tempo de execução públicas.  
  
```csharp
public class InterceptingBindingElement : BindingElement
{
}
```  
  
## <a name="adding-configuration-support"></a>Adicionando suporte à configuração  
 Para integrar com a configuração de associação, a biblioteca define um manipulador de seção de configuração como uma seção de extensão de elemento de associação. Os arquivos de configuração do cliente e do servidor devem registrar a extensão do elemento de associação com o sistema de configuração. Os implementadores que desejam expor seu elemento de ligação para o sistema de configuração podem derivar dessa classe.  
  
```csharp
public abstract class InterceptingElement : BindingElementExtensionElement 
{ 
    //... 
}
```  
  
## <a name="adding-policy"></a>Adicionando política  
 Para integrar com nosso sistema de política, `InterceptingBindingElement` implementa o IPolicyExportExtension para sinalizar que devemos participar da geração de políticas. Para dar suporte à política de importação em um cliente gerado, o usuário pode registrar uma classe derivada de `InterceptingBindingElementImporter` e substituir `CreateMessageInterceptor`() para gerar sua classe de `ChannelMessageInterceptor` habilitada para política.  
  
## <a name="example-droppable-message-inspector"></a>Exemplo: Inspetor de mensagem Dropper  
 Incluído no exemplo, há um exemplo de implementação de `ChannelMessageInspector` que descarta mensagens.  
  
```csharp
class DroppingServerElement : InterceptingElement  
{  
    protected override ChannelMessageInterceptor CreateMessageInterceptor()  
    {  
        return new DroppingServerInterceptor();  
    }  
}  
```  
  
 Você pode acessá-lo da configuração da seguinte maneira:  
  
```xml  
<configuration>  
    ...  
    <system.serviceModel>  
        ...  
        <extensions>  
            <bindingElementExtensions>  
                <add name="droppingInterceptor"   
                   type=  
          "Microsoft.ServiceModel.Samples.DroppingServerElement, library"/>  
            </bindingElementExtensions>  
        </extensions>  
    </system.serviceModel>  
</configuration>  
```  
  
 O cliente e o servidor usam essa seção de configuração recém-criada para inserir as fábricas personalizadas no nível mais baixo de suas pilhas de canal de tempo de execução (acima do nível de transporte).  
  
```xml  
<customBinding>  
  <binding name="sampleBinding">  
    <droppingInterceptor/>  
    <httpTransport/>  
  </binding>  
</customBinding>  
```  
  
 O cliente usa a biblioteca de `MessageInterceptor` para adicionar um cabeçalho personalizado a mensagens numeradas par. O serviço, por outro lado, usa `MessageInterceptor` biblioteca para descartar as mensagens que não têm esse cabeçalho especial.  
  
 Você deverá ver a seguinte saída do cliente depois de executar o serviço e, em seguida, o cliente.  
  
```console  
Reporting the next 10 wind speed  
100 kph  
Server dropped a message.  
90 kph  
80 kph  
Server dropped a message.  
70 kph  
60 kph  
Server dropped a message.  
50 kph  
40 kph  
Server dropped a message.  
30 kph  
20 kph  
Server dropped a message.  
10 kph  
Press ENTER to shut down client  
```  
  
 O cliente relata 10 velocidades de vento diferentes para o serviço, mas apenas marca metade delas com o cabeçalho especial.  
  
 No serviço, você deve ver a seguinte saída:  
  
```console  
Press ENTER to exit.  
Dangerous wind detected! Reported speed (90) is greater than 64 kph.  
Dangerous wind detected! Reported speed (70) is greater than 64 kph.  
5 wind speed reports have been received.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Instale o ASP.NET 4,0 usando o comando a seguir.  
  
    ```console  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
3. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
5. Execute o Service. exe primeiro e, em seguida, execute Client. exe e observe as janelas do console para saída.  
