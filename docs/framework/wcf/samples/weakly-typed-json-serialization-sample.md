---
title: Weakly-typed JSON Serialization Sample
ms.date: 03/30/2017
ms.assetid: 0b30e501-4ef5-474d-9fad-a9d559cf9c52
ms.openlocfilehash: 1450a0e46ade615769d7ffdc1006102772dbc977
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2019
ms.locfileid: "73424535"
---
# <a name="weakly-typed-json-serialization-sample"></a>Weakly-typed JSON Serialization Sample
Ao serializar um tipo definido pelo usuário para um determinado formato de conexão ou desserializar um formato de conexão de volta para um tipo definido pelo usuário, o tipo definido pelo usuário fornecido deve estar disponível no serviço e no cliente. Normalmente, para fazer isso, o atributo <xref:System.Runtime.Serialization.DataContractAttribute> é aplicado a esses tipos definidos pelo usuário e o atributo <xref:System.Runtime.Serialization.DataMemberAttribute> é aplicado aos seus membros. Esse mecanismo também se aplica ao trabalhar com objetos JavaScript Object Notation (JSON), conforme descrito no tópico [como serializar e desserializar dados JSON](../../../../docs/framework/wcf/feature-details/how-to-serialize-and-deserialize-json-data.md).  
  
 Em alguns cenários, um serviço ou cliente do Windows Communication Foundation (WCF) deve acessar objetos JSON gerados por um serviço ou cliente que está fora do controle do desenvolvedor. À medida que mais serviços Web expõem publicamente APIs JSON, pode se tornar impraticável para o desenvolvedor do WCF construir tipos locais definidos pelo usuário para desserializar objetos JSON arbitrários. Este exemplo fornece um mecanismo que permite que os desenvolvedores do WCF trabalhem com objetos JSON desserializados e arbitrários, sem criar tipos definidos pelo usuário. Isso é conhecido como *serialização de tipo fraco* de objetos JSON, pois o tipo no qual um objeto JSON é desserializado não é conhecido no momento da compilação.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Por exemplo, uma API de serviço Web pública retorna o seguinte objeto JSON, que descreve algumas informações sobre um usuário do serviço.  
  
```json  
{"personal": {"name": "Paul", "age": 23, "height": 1.7, "isSingle": true, "luckyNumbers": [5,17,21]}, "favoriteBands": ["Band ABC", "Band XYZ"]}  
```  
  
 Para desserializar esse objeto, um cliente WCF deve implementar os seguintes tipos definidos pelo usuário.  
  
```csharp  
[DataContract]  
 public class MemberProfile  
 {  
     [DataMember]  
     public PersonalInfo personal;  
  
     [DataMember]  
     public string[] favoriteBands;  
 }  
  
 [DataContract]  
 public class PersonalInfo  
 {  
     [DataMember]  
     public string name;  
  
     [DataMember]  
     public int age;  
  
     [DataMember]  
     public double height;  
  
     [DataMember]  
     public bool isSingle;  
  
     [DataMember]  
     public int[] luckyNumbers;  
 }  
```  
  
 Isso pode ser complicado, especialmente se o cliente tiver que lidar com mais de um tipo de objeto JSON.  
  
 O tipo de `JsonObject` fornecido por este exemplo apresenta uma representação de tipo fraco do objeto JSON desserializado. `JsonObject` se baseia no mapeamento natural entre objetos JSON e .NET Framework dicionários e o mapeamento entre matrizes JSON e matrizes de .NET Framework. O código a seguir mostra o tipo de `JsonObject`.  
  
```csharp  
// Instantiation of JsonObject json omitted  
  
string name = json["root"]["personal"]["name"];  
int age = json["root"]["personal"]["age"];  
double height = json["root"]["personal"]["height"];  
bool isSingle = json["root"]["personal"]["isSingle"];  
int[] luckyNumbers = {  
                                     json["root"]["personal"]["luckyNumbers"][0],  
                                     json["root"]["personal"]["luckyNumbers"][1],  
                                     json["root"]["personal"]["luckyNumbers"][2]   
                                 };  
string[] favoriteBands = {  
                                        json["root"]["favoriteBands"][0],  
                                        json["root"]["favoriteBands"][1]  
                                    };  
```  
  
 Observe que você pode "procurar" objetos JSON e matrizes sem a necessidade de declarar seu tipo no momento da compilação. Para obter uma explicação sobre o requisito para o objeto de `["root"]` de nível superior, consulte o tópico [mapeamento entre JSON e XML](../../../../docs/framework/wcf/feature-details/mapping-between-json-and-xml.md).  
  
> [!NOTE]
> A classe `JsonObject` é fornecida apenas como exemplo. Ele não foi totalmente testado e não deve ser usado em ambientes de produção. Uma implicação óbvia de serialização JSON de tipo fraco é a falta de segurança de tipos ao trabalhar com `JsonObject`.  
  
 Para usar o tipo de `JsonObject`, o contrato de operação do cliente deve usar <xref:System.ServiceModel.Channels.Message> como seu tipo de retorno.  
  
```csharp  
[ServiceContract]  
    interface IClientSideProfileService  
    {  
        // There is no need to write a DataContract for the complex type returned by the service.  
        // The client will use a JsonObject to browse the JSON in the received message.  
  
        [OperationContract]  
        [WebGet(ResponseFormat = WebMessageFormat.Json)]  
        Message GetMemberProfile();  
    }  
```  
  
 Em seguida, o `JsonObject` é instanciado conforme mostrado no código a seguir.  
  
```csharp  
// Code to instantiate IClientSideProfileService channel omitted…  
  
// Make a request to the service and obtain the Json response  
XmlDictionaryReader reader = channel.GetMemberProfile().GetReaderAtBodyContents();  
  
// Go through the Json as though it is a dictionary. There is no need to map it to a .NET CLR type.  
JsonObject json = new JsonObject(reader);  
```  
  
 O construtor de `JsonObject` usa um <xref:System.Xml.XmlDictionaryReader>, que é obtido por meio do método <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents%2A>. O leitor contém uma representação XML da mensagem JSON recebida pelo cliente. Para obter mais informações, consulte o tópico [mapeamento entre JSON e XML](../../../../docs/framework/wcf/feature-details/mapping-between-json-and-xml.md).  
  
 O programa produz a seguinte saída:  
  
```console  
Service listening at http://localhost:8000/.  
To view the JSON output from the sample, navigate to http://localhost:8000/GetMemberProfile  
This is Paul's page. I am 23 years old and I am 1.7 meters tall.  
I am single.  
My lucky numbers are 5, 17, and 21.  
My favorite bands are Band ABC and Band XYZ.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Crie a solução WeaklyTypedJson. sln, conforme descrito em [criando os exemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3. Execute a solução.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) para baixar todas as Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] amostras. Este exemplo está localizado no seguinte diretório.  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Scenario\Ajax\WeaklyTypedJson`  
