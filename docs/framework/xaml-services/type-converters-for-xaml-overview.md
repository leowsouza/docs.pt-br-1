---
title: Visão geral de conversores de tipo para XAML
ms.date: 03/30/2017
helpviewer_keywords:
- XAML [XAML Services], type converters
- XAML [XAML Services], TypeConverter
- type conversion for XAML [XAML Services]
ms.assetid: 51a65860-efcb-4fe0-95a0-1c679cde66b7
ms.openlocfilehash: b54731cc1aba1a47ed6b11f2bff5c596a53fd4b5
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458513"
---
# <a name="type-converters-for-xaml-overview"></a>Visão geral de conversores de tipo para XAML
Os conversores de tipo fornecem lógica para um gravador de objeto que converte de uma cadeia de caracteres na marcação XAML em objetos específicos em um grafo de objeto. Em .NET Framework serviços XAML, o conversor de tipo deve ser uma classe derivada de <xref:System.ComponentModel.TypeConverter>. Alguns conversores também dão suporte ao caminho de salvamento XAML e podem ser usados para serializar um objeto em um formulário de cadeia de caracteres na marcação de serialização. Este tópico descreve como e quando os conversores de tipo em XAML são invocados e fornece conselhos de implementação para as substituições de método de <xref:System.ComponentModel.TypeConverter>.  
  
<a name="type_conversion_concepts"></a>   
## <a name="type-conversion-concepts"></a>Conceitos de conversão de tipos  
 As seções a seguir explicam os conceitos básicos sobre como o XAML usa cadeias de caracteres e como os gravadores de objeto em .NET Framework serviços XAML usam conversores de tipo para processar alguns dos valores de cadeia de caracteres que são encontrados em uma fonte XAML.  
  
### <a name="xaml-and-string-values"></a>XAML e valores de cadeia de caracteres  
 Quando você define um valor de atributo em um arquivo XAML, o tipo inicial desse valor é uma cadeia de caracteres em um sentido geral e um valor de atributo de cadeia de caracteres em um sentido XML. Até mesmo outros primitivos como <xref:System.Double> são inicialmente cadeias de caracteres para um processador XAML.  
  
 Na maioria dos casos, um processador XAML precisa de duas informações para processar um valor de atributo. A primeira informação é o tipo de valor da propriedade que está sendo definido. Qualquer cadeia de caracteres que define um valor de atributo e que é processada em XAML deve, por fim, ser convertida ou resolvida para um valor desse tipo. Caso o valor seja um primitivo compreendido pelo analisador XAML (como um valor numérico), será tentada uma conversão direta da cadeia de caracteres. Se o valor do atributo fizer referência a uma enumeração, a cadeia de caracteres fornecida será verificada em busca de um nome correspondente a uma constante nomeada nessa enumeração. Se o valor não for um primitivo de analisador compreendido nem um nome constante de uma enumeração, o tipo aplicável deverá ser capaz de fornecer um valor ou uma referência com base em uma cadeia de caracteres convertida.  
  
> [!NOTE]
> As diretivas de linguagem XAML não usam conversores de tipo.  
  
### <a name="type-converters-and-markup-extensions"></a>Conversores de tipo e extensões de marcação  
 Os usos de extensão de marcação devem ser manipulados por um processador XAML antes de verificar o tipo de propriedade e outras considerações. Por exemplo, se uma propriedade que está sendo definida como um atributo normalmente tiver uma conversão de tipo, mas em um caso específico for definido por um uso de extensão de marcação, o comportamento de extensão de marcação será processado primeiro. Uma situação comum em que uma extensão de marcação é necessária é fazer uma referência a um objeto que já existe. Para esse cenário, um conversor de tipo sem estado só pode gerar uma nova instância, o que pode não ser desejável. Para obter mais informações sobre extensões de marcação, consulte [Markup Extensions for XAML Overview](markup-extensions-for-xaml-overview.md).  
  
### <a name="native-type-converters"></a>Conversores de tipos nativos  
 Nas implementações de serviços XAML do WPF e do .NET, há certos tipos de CLR que têm manipulação de conversão de tipo nativo, no entanto, esses tipos de CLR não são considerados como primitivos. Um exemplo desse tipo é <xref:System.DateTime>. Um motivo para isso é como a arquitetura de .NET Framework funciona: o tipo <xref:System.DateTime> é definido em mscorlib, a biblioteca mais básica no .NET. <xref:System.DateTime> não tem permissão para ser atribuída a um atributo proveniente de outro assembly que introduz uma dependência (<xref:System.ComponentModel.TypeConverterAttribute> é do sistema); Portanto, o mecanismo de descoberta do conversor de tipos usual por atribuição não pode ser suportado. Em vez disso, o analisador XAML tem uma lista de tipos que precisam de processamento nativo e processa esses tipos de forma semelhante a como os primitivos verdadeiros são processados. No caso de <xref:System.DateTime>, esse processamento envolve uma chamada para <xref:System.DateTime.Parse%2A>.  
  
<a name="Implementing_a_Type_Converter"></a>   
## <a name="implementing-a-type-converter"></a>Implementando um conversor de tipos  
 As seções a seguir discutem a API da classe <xref:System.ComponentModel.TypeConverter>.  
  
### <a name="typeconverter"></a>TypeConverter  
 Em .NET Framework serviços XAML, todos os conversores de tipo que são usados para fins XAML são classes que derivam da classe base <xref:System.ComponentModel.TypeConverter>. A classe <xref:System.ComponentModel.TypeConverter> existia em versões do .NET Framework antes da existência de XAML; um dos cenários de <xref:System.ComponentModel.TypeConverter> originais era fornecer conversão de cadeia de caracteres para editores de propriedade em designers visuais.  
  
 Para o XAML, a função de <xref:System.ComponentModel.TypeConverter> é expandida. Para fins de XAML, <xref:System.ComponentModel.TypeConverter> é a classe base para fornecer suporte para determinadas conversões de cadeia de caracteres e de cadeia de caracteres. From-String habilita a análise de um valor de atributo de cadeia de caracteres do XAML. To-String pode permitir o processamento de um valor de tempo de execução de uma determinada propriedade de objeto de volta em um atributo em XAML para serialização.  
  
 <xref:System.ComponentModel.TypeConverter> define quatro membros que são relevantes para converter para-String e from-String para fins de processamento XAML:  
  
- <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A>  
  
- <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A>  
  
- <xref:System.ComponentModel.TypeConverter.ConvertTo%2A>  
  
- <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A>  
  
 Desses membros, o método mais importante é <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A>, que converte a cadeia de caracteres de entrada para o tipo de objeto necessário. O método <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> pode ser implementado para converter um intervalo maior de tipos no tipo de destino pretendido do conversor. Portanto, ele pode atender a finalidades que se estendem além do XAML, como suporte a conversões de tempo de execução. No entanto, para uso de XAML, somente o caminho de código que pode processar uma entrada de <xref:System.String> é importante.  
  
 O segundo método mais importante é <xref:System.ComponentModel.TypeConverter.ConvertTo%2A>. Se um aplicativo for convertido em uma representação de marcação (por exemplo, se for salvo em XAML como um arquivo), <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> estará envolvido no cenário maior de um gravador de texto XAML produzir uma representação de marcação. Nesse caso, o caminho de código importante para XAML é quando o chamador passa uma `destinationType` de <xref:System.String>.  
  
 <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> e <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A> são métodos de suporte que são usados quando um serviço consulta os recursos da implementação do <xref:System.ComponentModel.TypeConverter>. Esses métodos devem ser implementados para retornar o `true` para casos específicos de tipo aos quais os métodos de conversão equivalentes do seu conversor dão suporte. Para fins de XAML, isso geralmente significa o tipo de <xref:System.String>.  
  
### <a name="culture-information-and-type-converters-for-xaml"></a>Informações de cultura e conversores de tipos para XAML  
 Cada implementação de <xref:System.ComponentModel.TypeConverter> pode interpretar exclusivamente o que é uma cadeia de caracteres válida para uma conversão e também pode usar ou ignorar a descrição de tipo que é passada como parâmetros. Uma consideração importante para a conversão de tipo de cultura e XAML é a seguinte: embora o uso de cadeias de caracteres localizáveis como valores de atributo seja suportado pelo XAML, você não pode usar essas cadeias de caracteres localizáveis como entrada do conversor de tipo com requisitos de cultura específicos Essa limitação é porque os conversores de tipo para valores de atributo XAML envolvem um comportamento de processamento XAML-linguagem necessariamente fixo que usa `en-US` cultura. Para obter mais informações sobre os motivos de design para essa restrição, consulte a especificação da linguagem XAML ([\[MS-XAML\]](https://go.microsoft.com/fwlink/?LinkId=114525)) ou a [globalização do WPF e visão geral da localização](../wpf/advanced/wpf-globalization-and-localization-overview.md).  
  
 Como um exemplo em que a cultura pode ser um problema, algumas culturas usam uma vírgula em vez de um ponto como o delimitador de ponto decimal para números na forma de cadeia de caracteres. Esse uso colide com o comportamento que muitos conversores de tipo existentes têm, que é usar uma vírgula como um delimitador. Passar uma cultura por meio de `xml:lang` no XAML ao redor não resolve o problema.  
  
### <a name="implementing-convertfrom"></a>Implementando ConvertFrom  
 Para ser utilizável como uma implementação <xref:System.ComponentModel.TypeConverter> que dá suporte a XAML, o método <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> para esse conversor deve aceitar uma cadeia de caracteres como o parâmetro `value`. Se a cadeia de caracteres estiver em um formato válido e puder ser convertida pela implementação de <xref:System.ComponentModel.TypeConverter>, o objeto retornado deverá dar suporte a uma conversão para o tipo esperado pela propriedade. Caso contrário, a implementação de <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> deve retornar `null`.  
  
 Cada implementação de <xref:System.ComponentModel.TypeConverter> pode interpretar exclusivamente o que constitui uma cadeia de caracteres válida para uma conversão e também pode usar ou ignorar a descrição do tipo ou contextos de cultura que são passados como parâmetros. No entanto, o processamento XAML do WPF pode não passar valores para o contexto de descrição de tipo em todos os casos e também pode não passar cultura com base em `xml:lang`.  
  
> [!NOTE]
> Não use as chaves ({}), especificamente a chave de abertura ({), como um elemento de seu formato de cadeia de caracteres. Esses caracteres são reservados como entrada e saída para uma sequência de extensão de marcação.  
  
 É apropriado lançar uma exceção quando seu conversor de tipo deve ter acesso a um serviço XAML do gravador de objeto de serviços XAML .NET Framework, mas a chamada <xref:System.IServiceProvider.GetService%2A> feita no contexto não retorna esse serviço.  
  
### <a name="implementing-convertto"></a>Implementando ConvertTo  
 <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> é potencialmente usado para suporte de serialização. O suporte de serialização por meio de <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> para seu tipo personalizado e seu conversor de tipo não é um requisito absoluto. No entanto, se você estiver implementando um controle ou usando a serialização do como parte dos recursos ou do design de sua classe, deverá implementar <xref:System.ComponentModel.TypeConverter.ConvertTo%2A>.  
  
 Para ser utilizável como uma implementação <xref:System.ComponentModel.TypeConverter> que dá suporte a XAML, o método <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> para esse conversor deve aceitar uma instância do tipo (ou um valor) com suporte como o parâmetro `value`. Quando o parâmetro `destinationType` é do tipo <xref:System.String>, o objeto retornado deve ser capaz de ser convertido como <xref:System.String>. A cadeia de caracteres retornada deve representar um valor serializado de `value`. Idealmente, o formato de serialização que você escolher deve ser capaz de gerar o mesmo valor como se essa cadeia de caracteres fosse passada para a implementação de <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> do mesmo conversor, sem perda significativa de informações.  
  
 Se o valor não puder ser serializado ou o conversor não oferecer suporte à serialização, a implementação de <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> deverá retornar `null` e poderá gerar uma exceção. No entanto, se você lançar exceções, deverá informar a incapacidade de usar essa conversão como parte de sua implementação de <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> para que a prática recomendada de verificação com <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> primeiro para evitar exceções seja suportada.  
  
 Se o parâmetro `destinationType` não for do tipo <xref:System.String>, você poderá escolher sua própria manipulação de conversor. Normalmente, você reverte para o tratamento de implementação base, que na base <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> gera uma exceção específica.  
  
 É apropriado lançar uma exceção quando seu conversor de tipo deve ter acesso a um serviço XAML do gravador de objeto de serviços XAML .NET Framework, mas a chamada <xref:System.IServiceProvider.GetService%2A> feita no contexto não retorna esse serviço.  
  
### <a name="implementing-canconvertfrom"></a>Implementando CanConvertFrom  
 Sua implementação de <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A> deve retornar `true` para `sourceType` do tipo <xref:System.String> e, caso contrário, adiar para a implementação de base. Não lance exceções de <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A>.  
  
### <a name="implementing-canconvertto"></a>Implementando CanConvertTo  
 Sua implementação de <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> deve retornar `true` para `destinationType` do tipo <xref:System.String>e, caso contrário, adiar para a implementação base. Não lance exceções de <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A>.  
  
<a name="Applying_the_TypeConverterAttribute"></a>   
## <a name="applying-the-typeconverterattribute"></a>Aplicando o TypeConverterAttribute  
 Para que o conversor de tipo personalizado seja usado como o conversor de tipo atuante para uma classe personalizada por .NET Framework serviços XAML, você deve aplicar o <xref:System.ComponentModel.TypeConverterAttribute> à sua definição de classe. O <xref:System.ComponentModel.TypeConverterAttribute.ConverterTypeName%2A> especificado por meio do atributo deve ser o nome do tipo do conversor de tipo personalizado. Se você aplicar esse atributo, quando um processador XAML processar valores em que o tipo de propriedade usa seu tipo de classe personalizada, ele poderá inserir cadeias de caracteres e retornar instâncias de objeto.  
  
 Também é possível fornecer um conversor de tipos por propriedade. Em vez de aplicar um <xref:System.ComponentModel.TypeConverterAttribute> à definição de classe, aplique-o a uma definição de propriedade (a definição principal, não o `get`/`set` implementações dentro dele). O tipo da propriedade deve corresponder ao tipo que é processado pelo conversor de tipos personalizado. Com esse atributo aplicado, quando um processador XAML manipula valores dessa propriedade, ele pode processar cadeias de caracteres de entrada e retornar instâncias de objeto. A técnica de conversor de tipo por propriedade é particularmente útil se você optar por usar um tipo de Propriedade do Microsoft .NET Framework ou de alguma outra biblioteca em que você não pode controlar a definição de classe e não pode aplicar um <xref:System.ComponentModel.TypeConverterAttribute> lá.  
  
 Para fornecer um comportamento de conversão de tipo para um membro anexado personalizado, aplique <xref:System.ComponentModel.TypeConverterAttribute> ao método acessador de `Get` do padrão de implementação do membro anexado.  
  
<a name="accessing_service_provider_context_from_a_markup_extension_implementation"></a>   
## <a name="accessing-service-provider-context-from-a-markup-extension-implementation"></a>Acessando o contexto do provedor de serviço de uma implementação de extensão de marcação  
 Os serviços disponíveis são os mesmos para qualquer conversor de valor. A diferença está em como cada conversor de valor recebe o contexto de serviço. O acesso aos serviços e aos serviços disponíveis é documentado no tópico [tipos de conversores e extensões de marcação para XAML](type-converters-and-markup-extensions-for-xaml.md).  
  
<a name="type_converters_in_the_xaml_node_stream"></a>   
## <a name="type-converters-in-the-xaml-node-stream"></a>Conversores de tipo no fluxo do nó XAML  
 Se você estiver trabalhando com um fluxo de nó XAML, a ação ou o resultado final de um conversor de tipo ainda não será executado. Em um caminho de carga, a cadeia de caracteres de atributo que eventualmente precisa ser convertida de tipo para ser carregada permanece como um valor de texto dentro de um membro inicial e membro final. O conversor de tipo que é eventualmente necessário para essa operação pode ser determinado usando a propriedade <xref:System.Xaml.XamlMember.TypeConverter%2A?displayProperty=nameWithType>. No entanto, a obtenção de um valor válido de <xref:System.Xaml.XamlMember.TypeConverter%2A?displayProperty=nameWithType> se baseia em ter um contexto de esquema XAML, que pode acessar essas informações por meio do membro subjacente ou o tipo de valor de objeto que o membro usa. Invocar o comportamento de conversão de tipo também requer o contexto de esquema XAML porque isso requer o mapeamento de tipo e a criação de uma instância de conversor.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ComponentModel.TypeConverterAttribute>
- [Conversores de tipo e extensões de marcação para XAML](type-converters-and-markup-extensions-for-xaml.md)
- [Visão geral de XAML (WPF)](../../desktop-wpf/fundamentals/xaml.md)
