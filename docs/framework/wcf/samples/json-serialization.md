---
title: Exemplo de DataContractJsonSerializer
ms.date: 03/30/2017
ms.assetid: 3c2c4747-7510-4bdf-b4fe-64f98428ef4a
ms.openlocfilehash: 509f80812bb815e4fa56fa3ebdc9236ac0622ace
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2019
ms.locfileid: "74141826"
---
# <a name="datacontractjsonserializer-sample"></a>Exemplo de DataContractJsonSerializer

> [!NOTE]
> Este exemplo é para <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>. Para a maioria dos cenários que envolvem a serialização e desserialização do JSON, recomendamos as ferramentas no [namespace System. Text. JSON](../../../standard/serialization/system-text-json-overview.md). 

O <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> dá suporte aos mesmos tipos que o <xref:System.Runtime.Serialization.DataContractSerializer>. O formato de dados JSON é especialmente útil ao escrever aplicativos Web de estilo AJAX (Asynchronous JavaScript and XML). O suporte ao AJAX no Windows Communication Foundation (WCF) é otimizado para uso com o ASP.NET AJAX por meio do controle ScriptManager. Para obter exemplos de como usar o Windows Communication Foundation (WCF) com o ASP.NET AJAX, consulte os [exemplos do AJAX](ajax.md).  
  
Os procedimentos de instalação e as instruções de compilação para esse exemplo estão localizadas no final deste tópico.  
  
Este exemplo usa um contrato de dados `Person` para demonstrar a serialização e a desserialização.  

```csharp
[DataContract]
class Person
{
    [DataMember]
    internal string name;

    [DataMember]
    internal int age;
}
```

 Para serializar uma instância do tipo `Person` para JSON, crie primeiro o <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> e use o método `WriteObject` para gravar dados JSON em um fluxo.  

```csharp
Person p = new Person();
//Set up Person object...
MemoryStream stream1 = new MemoryStream();
DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(Person));
ser.WriteObject(stream1, p);
```

 O fluxo de memória contém dados JSON válidos.
  
```json  
{"age":42,"name":"John"}  
```  
  
 O exemplo demonstra a desserialização de dados JSON em um objeto. Em seguida, você volta o fluxo e chama `ReadObject`.  

```csharp
Person p2 = (Person)ser.ReadObject(stream1);
```

 Examinar o objeto `p2` revela que os dados JSON foram desserializados corretamente.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) para baixar todas as Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] amostras. Este exemplo está localizado no seguinte diretório.  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Ajax\JsonSerialization`  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar e executar o exemplo  
  
1. Crie a solução JsonSerialization. sln, conforme descrito em [criando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
2. Execute o aplicativo de console resultante.  
