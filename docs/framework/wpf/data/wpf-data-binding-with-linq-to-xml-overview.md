---
title: Associação de dados do WPF com LINQ to XML
ms.date: 10/22/2019
ms.topic: conceptual
ms.openlocfilehash: 53aba3295b98ae4a476b321cb585e1bbbdd45dad
ms.sourcegitcommit: 5a28f8eb071fcc09b045b0c4ae4b96898673192e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73197383"
---
# <a name="overview-of-wpf-data-binding-with-linq-to-xml"></a>Visão geral da Associação de dados do WPF com LINQ to XML

Este artigo apresenta os recursos de ligação de dados dinâmicos no namespace <xref:System.Xml.Linq>. Esses recursos podem ser usados como uma fonte de dados para elementos de interface do usuário no WPF (Windows Presentation Foundation). Este cenário baseia-se em *propriedades dinâmicas* especiais do <xref:System.Xml.Linq.XAttribute?displayProperty=fullName> e do <xref:System.Xml.Linq.XElement?displayProperty=fullName>.

## <a name="xaml-and-linq-to-xml"></a>XAML e LINQ to XML

A linguagem XAML é um dialeto XML criado pela Microsoft para dar suporte a tecnologias do .NET. Ela é usada em WPF para representar elementos de interface do usuário e recursos relacionados, como associação de eventos e dados. No Windows Workflow Foundation, o XAML é usado para representar a estrutura do programa, como o controle de programa (*fluxos de trabalho*). O XAML permite que os aspectos declarativos de uma tecnologia sejam separados do código procedural relacionado que define o comportamento mais individualizado de um programa.

Há duas maneiras de o XAML e o LINQ to XML poderem interagir:

- Como os arquivos XAML são XMLs bem-formados, podem ser consultados e manipulados pelas tecnologias XML como LINQ to XML.

- Como as consultas LINQ to XML representam uma fonte de dados, elas podem ser usadas como uma fonte de dados para vinculação de dados para elementos de interface de usuário do WPF.

Esta documentação descreve o segundo cenário.

## <a name="data-binding-in-the-windows-presentation-foundation"></a>Associação de dados no Windows Presentation Foundation

A vinculação de dados do WPF permite que um elemento de interface de usuário associe uma de suas propriedades a uma fonte de dados. Um exemplo simples disso é um <xref:System.Windows.Controls.Label> cujo texto apresenta o valor de uma propriedade pública em um objeto definido pelo usuário. A vinculação de dados do WPF depende dos seguintes componentes:

|Componente|Descrição|
|---------------|-----------------|
|Destino de associação|O elemento de interface do usuário a ser associado com a fonte de dados. Os elementos visuais no WPF são derivados da classe <xref:System.Windows.UIElement>.|
|Propriedade de destino|A *propriedade de dependência* do destino da associação que reflete o valor da fonte da vinculação de dados. As propriedades de dependência têm suporte direto pela classe <xref:System.Windows.DependencyObject>, da qual <xref:System.Windows.UIElement>.|
|Fonte de associação|O objeto de origem para um ou mais valores que são fornecidos para o elemento da interface de usuário para apresentação. O WPF oferece suporte automático aos seguintes tipos como fontes de associação: objetos CLR, objetos de dados ADO.NET, dados XML (de consultas XPath ou LINQ to XML) ou outro <xref:System.Windows.DependencyObject>.|
|Caminho de origem|A propriedade da fonte de associação que resolve para o valor ou conjunto de valores que devem ser associados.|

Uma propriedade de dependência é um conceito específico do WPF que representa uma propriedade dinamicamente computada de um elemento de interface do usuário. Por exemplo, as propriedades de dependência geralmente têm valores padrão ou valores que são fornecidos por um elemento pai. Essas propriedades especiais têm o suporte de instâncias da classe <xref:System.Windows.DependencyProperty> (e não de campos com propriedades padrão). Para obter mais informações, consulte [Visão geral sobre propriedades de dependência](../advanced/dependency-properties-overview.md).

### <a name="dynamic-data-binding-in-wpf"></a>Vinculação de dados dinâmicas no WPF

Por padrão, a vinculação de dados ocorre somente quando o elemento de interface do usuário de destino é inicializado. Isso é chamado de associação *única*. Para a maioria das finalidades, isso é insuficiente; normalmente, uma solução de vinculação de dados exige que as alterações sejam propagadas dinamicamente em tempo de execução usando um destes procedimentos:

- A associação *unidirecional* faz as alterações em um lado serem propagadas automaticamente. Mais comumente, as alterações da origem são refletidas no destino, mas o inverso muitas vezes pode ser útil.

- Na associação *bidirecional*, as alterações da origem são propagadas automaticamente para o destino, e as alterações para o destino são propagadas automaticamente para a origem.

Para que a associação unidirecional ou bidirecional ocorra, a origem deve implementar um mecanismo de notificação de mudança, por exemplo, implementando a interface de <xref:System.ComponentModel.INotifyPropertyChanged> ou usando um padrão *PropertyNameChanged* para cada propriedade com suporte.

Para obter mais informações sobre vinculação de dados no WPF, consulte [Vinculação de dados (WPF)](/dotnet/framework/wpf/data/data-binding-wpf).

## <a name="dynamic-properties-in-linq-to-xml-classes"></a>Propriedades dinâmicas em classes LINQ to XML

A maioria das classes LINQ to XML não se qualificam como fontes de dados dinâmicos do WPF adequadas. Algumas das informações mais úteis estão disponíveis somente através de métodos, e não propriedades, e as propriedades nessas classes não implementam notificações de alteração. Para oferecer suporte à vinculação de dados do WPF, o LINQ to XML expõe um conjunto de *propriedades dinâmicas*.

Essas propriedades dinâmicas são as propriedades especiais em tempo de execução que duplicam a funcionalidade dos métodos existentes e as propriedades nas classes <xref:System.Xml.Linq.XAttribute> e de <xref:System.Xml.Linq.XElement>. Elas foram adicionados às classes exclusivamente para habilitá-las para atuar como fontes de dados dinâmicas para WPF. Para atender essa necessidade, todas essas propriedades dinâmicas implementam notificações de alterações. Uma referência detalhada sobre essas propriedades dinâmicas é fornecida na próxima seção, [Propriedades dinâmicas LINQ to XML](linq-to-xml-dynamic-properties.md).

> [!NOTE]
> Muitas das propriedades públicas padrão, localizadas em várias classes no namespace <xref:System.Xml.Linq>, podem ser usadas para a vinculação de dados única. No entanto, lembre-se de que nem a origem ou o destino serão atualizados dinamicamente neste esquema.

### <a name="access-dynamic-properties"></a>Acessar propriedades dinâmicas

As propriedades dinâmicas nas classes <xref:System.Xml.Linq.XAttribute> e <xref:System.Xml.Linq.XElement> não podem ser acessadas como propriedades padrão. Por exemplo, em linguagens compatíveis com CLR, como C#, elas não podem ser:

- Acessadas diretamente em tempo de compilação. As propriedades dinâmicas são invisíveis para o compilador e o Visual Studio IntelliSense.

- Descobertas ou acessadas em tempo de execução usando reflexão .NET. Mesmo em tempo de execução, elas não são propriedades no sentido básico de CLR.

No C#, as propriedades dinâmicas podem ser acessadas somente em tempo de execução através de recursos fornecidos pelo namespace <xref:System.ComponentModel>.

Ao contrário, no entanto, em uma origem XML as propriedades dinâmicas podem ser acessadas através de uma notação simples da seguinte forma:

```xml
<object>.<dynamic-property>
```

As propriedades dinâmicas para essas duas classes resolvem para um valor que pode ser usado diretamente, ou para um indexador que deve ser fornecido com um índice para obter o valor resultante ou uma coleção de valores. A última sintaxe utiliza o formato:

```xml
<object>.<dynamic-property>[<index-value>]
```

Para obter mais informações, consulte [Propriedades dinâmicas LINQ to XML](linq-to-xml-dynamic-properties.md).

Para implementar a associação dinâmica de WPF, as propriedades dinâmicas serão usadas com os recursos fornecidos pelo namespace <xref:System.Windows.Data>, especialmente a classe <xref:System.Windows.Data.Binding>.

## <a name="see-also"></a>Consulte também

- [Vinculação de dados de WPF com LINQ to XML](wpf-data-binding-with-linq-to-xml-overview.md)
- [Propriedades dinâmicas LINQ to XML](linq-to-xml-dynamic-properties.md)
- [XAML no WPF](../advanced/xaml-in-wpf.md)
- [Associação de dados (WPF)](/dotnet/framework/wpf/data/data-binding-wpf)
- [Usando a marcação de fluxo de trabalho](https://go.microsoft.com/fwlink/?LinkId=98685)
