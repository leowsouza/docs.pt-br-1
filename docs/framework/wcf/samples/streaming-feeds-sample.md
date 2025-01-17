---
title: Exemplo de Streaming Feeds
ms.date: 03/30/2017
ms.assetid: 1f1228c0-daaa-45f0-b93e-c4a158113744
ms.openlocfilehash: ede1dbb4f5c682b8182dda4888a9cbd373b95dd8
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976368"
---
# <a name="streaming-feeds-sample"></a>Exemplo de Streaming Feeds
Este exemplo demonstra como gerenciar feeds de distribuição que contêm um grande número de itens. No servidor, o exemplo demonstra como atrasar a criação de objetos <xref:System.ServiceModel.Syndication.SyndicationItem> individuais no feed até imediatamente antes que o item seja gravado no fluxo de rede.  
  
 No cliente, o exemplo mostra como um formatador de feed de agregação personalizado pode ser usado para ler itens individuais do fluxo de rede para que o feed que está sendo lido nunca seja totalmente armazenado em buffer na memória.  
  
 Para demonstrar melhor o recurso de streaming da API de distribuição, este exemplo usa um cenário um pouco improvável, no qual o servidor expõe um feed que contém um número infinito de itens. Nesse caso, o servidor continua gerando novos itens no feed até determinar que o cliente leu um número especificado de itens do feed (por padrão, 10). Para simplificar, o cliente e o servidor são implementados no mesmo processo e usam um objeto `ItemCounter` compartilhado para manter o controle de quantos itens o cliente produziu. O tipo de `ItemCounter` existe somente para permitir que o cenário de exemplo seja encerrado corretamente e não seja um elemento básico do padrão que está sendo demonstrado.  
  
 A demonstração usa os iteradores C# visuais (usando a construção de palavra-chave `yield return`). Para obter mais informações sobre iteradores, consulte o tópico "usando iteradores" no MSDN.  
  
## <a name="service"></a>Serviço  
 O serviço implementa um contrato de <xref:System.ServiceModel.Web.WebGetAttribute> básico que consiste em uma operação, conforme mostrado no código a seguir.  
  
```csharp  
[ServiceContract]  
interface IStreamingFeedService  
{  
    [WebGet]  
    [OperationContract]  
    Atom10FeedFormatter StreamedFeed();  
}  
```  
  
 O serviço implementa esse contrato usando uma classe `ItemGenerator` para criar um fluxo potencialmente infinito de instâncias de <xref:System.ServiceModel.Syndication.SyndicationItem> usando um iterador, conforme mostrado no código a seguir.  
  
```csharp
class ItemGenerator  
{  
    public IEnumerable<SyndicationItem> GenerateItems()  
    {  
        while (counter.GetCount() < maxItemsRead)  
        {  
            itemsReturned++;  
            yield return CreateNextItem();  
        }  
  
    }  
    ...  
}  
```  
  
 Quando a implementação do serviço cria o feed, a saída de `ItemGenerator.GenerateItems()` é usada em vez de uma coleção de itens em buffer.  
  
```csharp
public Atom10FeedFormatter StreamedFeed()  
{  
    SyndicationFeed feed = new SyndicationFeed("Streamed feed", "Feed to test streaming", null);  
    //Generate an infinite stream of items. Both the client and the service share  
    //a reference to the ItemCounter, which allows the sample to terminate  
    //execution after the client has read 10 items from the stream  
    ItemGenerator itemGenerator = new ItemGenerator(this.counter, 10);  
  
    feed.Items = itemGenerator.GenerateItems();  
    return feed.GetAtom10Formatter();  
}  
```  
  
 Como resultado, o fluxo de itens nunca é totalmente armazenado em buffer na memória. Você pode observar esse comportamento definindo um ponto de interrupção na instrução `yield return` dentro do método `ItemGenerator.GenerateItems()` e observando que esse ponto de interrupção é encontrado pela primeira vez depois que o serviço retornou o resultado do método `StreamedFeed()`.  
  
## <a name="client"></a>Cliente  
 O cliente neste exemplo usa uma implementação de <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> personalizada que atrasa a materialização de itens individuais no feed em vez de armazenar em buffer na memória. A instância de `StreamedAtom10FeedFormatter` personalizada é usada da seguinte maneira.  
  
```csharp  
XmlReader reader = XmlReader.Create("http://localhost:8000/Service/Feeds/StreamedFeed");  
StreamedAtom10FeedFormatter formatter = new StreamedAtom10FeedFormatter(counter);  
  
SyndicationFeed feed = formatter.ReadFrom(reader);  
```  
  
 Normalmente, uma chamada para <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter.ReadFrom%28System.Xml.XmlReader%29> não retorna até que todo o conteúdo do feed tenha sido lido da rede e armazenado em buffer na memória. No entanto, o objeto `StreamedAtom10FeedFormatter` substitui <xref:System.ServiceModel.Syndication.Atom10FeedFormatter.ReadItems%28System.Xml.XmlReader%2CSystem.ServiceModel.Syndication.SyndicationFeed%2CSystem.Boolean%40%29> para retornar um iterador em vez de uma coleção em buffer, conforme mostrado no código a seguir.  
  
```csharp  
protected override IEnumerable<SyndicationItem> ReadItems(XmlReader reader, SyndicationFeed feed, out bool areAllItemsRead)  
{  
    areAllItemsRead = false;  
    return DelayReadItems(reader, feed);  
}  
  
private IEnumerable<SyndicationItem> DelayReadItems(XmlReader reader, SyndicationFeed feed)  
{  
    while (reader.IsStartElement("entry", "http://www.w3.org/2005/Atom"))  
    {  
        yield return this.ReadItem(reader, feed);  
    }  
  
    reader.ReadEndElement();  
}  
```  
  
 Como resultado, cada item não é lido da rede até que o aplicativo cliente que atravessa os resultados de `ReadItems()` esteja pronto para usá-lo. Você pode observar esse comportamento definindo um ponto de interrupção na instrução `yield return` dentro de `StreamedAtom10FeedFormatter.DelayReadItems()` e percebendo que esse ponto de interrupção é encontrado pela primeira vez após a chamada para `ReadFrom()` ser concluída.  
  
 As instruções a seguir mostram como compilar e executar o exemplo. Observe que, embora o servidor pare de gerar itens depois que o cliente leu 10 itens, a saída mostra que o cliente lê significativamente mais de 10 itens. Isso ocorre porque a associação de rede usada pelo exemplo transmite dados em segmentos de quatro kilobytes (KB). Como tal, o cliente recebe 4 KB de dados do item antes de ter a oportunidade de ler até mesmo um item. Esse comportamento é normal (o envio de dados HTTP transmitidos em segmentos de tamanho razoável aumenta o desempenho).  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para compilar a C# edição do ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) para baixar todas as Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] amostras. Este exemplo está localizado no seguinte diretório.  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Syndication\StreamingFeeds`  
  
## <a name="see-also"></a>Consulte também

- [Feed de diagnóstico independente](../../../../docs/framework/wcf/samples/stand-alone-diagnostics-feed-sample.md)
