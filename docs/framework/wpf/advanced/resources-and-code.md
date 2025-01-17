---
title: Recursos e código
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- keys [WPF], using objects as
- resources [WPF], accessing from procedural code
- procedural code [WPF], creating resources with
- procedural code [WPF], accessing resources from
- resources [WPF], creating with procedural code
ms.assetid: c1cfcddb-e39c-41c8-a7f3-60984914dfae
ms.openlocfilehash: 3d504467c137c1e3f494e120217957661f4e75a3
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458760"
---
# <a name="resources-and-code"></a>Recursos e código
Esta visão geral se concentra em como recursos de [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] podem ser acessados ou criados usando o código em vez da sintaxe [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]. Para obter mais informações sobre o uso geral de recursos e sobre os recursos da perspectiva da sintaxe [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)], consulte [Recursos XAML](xaml-resources.md).  

<a name="accessing"></a>   
## <a name="accessing-resources-from-code"></a>Acessando recursos do código  
 As chaves que identificam os recursos se eles forem definidos pelo [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] também serão usadas para recuperar recursos específicos se você solicitá-los no código. A maneira mais simples de recuperar um recurso do código é chamar o <xref:System.Windows.FrameworkElement.FindResource%2A> ou o método <xref:System.Windows.FrameworkElement.TryFindResource%2A> de objetos de nível de estrutura em seu aplicativo. A diferença comportamental entre esses métodos é o que acontece caso a chave solicitada não seja encontrada. <xref:System.Windows.FrameworkElement.FindResource%2A> gera uma exceção; <xref:System.Windows.FrameworkElement.TryFindResource%2A> não gerará uma exceção, mas retornará `null`. Cada método usa a chave do recurso como um parâmetro de entrada e retorna um objeto que não é fortemente tipado. Normalmente, uma chave de recurso é uma cadeia de caracteres, mas há usos ocasionais em que esse não é o caso. Consulte a seção [Usando objetos como chaves](#objectaskey) para obter detalhes. Normalmente, você converteria o objeto retornado para o tipo exigido pela propriedade que você está definindo ao solicitar o recurso. A lógica de pesquisa para resolução de recurso do código é a mesma que no caso [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] da referência de recurso dinâmico. A pesquisa por recursos é iniciada do elemento de chamada e continua para os elementos pai sucessivos na árvore lógica. A pesquisa segue adiante para os recursos de aplicativo, temas e recursos de sistema, se necessário. Uma solicitação de código para um recurso considerará as alterações em runtime nos dicionários de recursos que podem ter sido feitos após o dicionário de recursos ter sido carregado do [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)], e também alterações de recursos de sistema em tempo real.  
  
 Veja a seguir um breve exemplo de código que localiza um recurso por chave e usa o valor retornado para definir uma propriedade, implementada como um manipulador de eventos <xref:System.Windows.Controls.Primitives.ButtonBase.Click>.  
  
 [!code-csharp[PropertiesOvwSupport#ResourceProceduralGet](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertiesOvwSupport/CSharp/page3.xaml.cs#resourceproceduralget)]
 [!code-vb[PropertiesOvwSupport#ResourceProceduralGet](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertiesOvwSupport/visualbasic/page3.xaml.vb#resourceproceduralget)]  
  
 Um método alternativo para atribuir uma referência de recurso é <xref:System.Windows.FrameworkElement.SetResourceReference%2A>. Este método utiliza dois parâmetros: a chave do recurso e o identificador de uma determinada propriedade de dependência que está presente na instância do elemento a que o valor do recurso deve ser atribuído. Funcionalmente, esse método é igual e tem a vantagem de não exigir nenhuma conversão de valores retornados.  
  
 Ainda outra maneira de acessar recursos de forma programática é acessar o conteúdo da propriedade <xref:System.Windows.FrameworkElement.Resources%2A> como um dicionário. Acessando o dicionário contido por esta propriedade, você também pode adicionar novos recursos a coleções existentes, verificar se um nome de chave já existe na coleção e outras operações de dicionário/coleção. Se você estiver escrevendo um aplicativo [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] inteiramente no código, também poderá criar a coleção inteira no código, atribuir chaves a ele e, em seguida, atribuir a coleção concluída à propriedade <xref:System.Windows.FrameworkElement.Resources%2A> de um elemento estabelecido. Isso será descrito na próxima seção.  
  
 Você pode indexar em qualquer coleção de <xref:System.Windows.FrameworkElement.Resources%2A> determinada, usando uma chave específica como o índice, mas deve estar ciente de que o acesso ao recurso dessa maneira não segue as regras de tempo de execução normal da resolução de recursos. Você está acessando apenas aquela coleção em particular. A pesquisa de recursos não percorrerá o escopo até a raiz ou o aplicativo se nenhum objeto válido tiver sido encontrado na chave solicitada. No entanto, essa abordagem pode ter vantagens de desempenho em alguns casos, precisamente porque o escopo da pesquisa no caso da chave é mais restrito. Consulte a classe <xref:System.Windows.ResourceDictionary> para obter mais detalhes sobre como trabalhar diretamente com o dicionário de recursos.  
  
<a name="creating"></a>   
## <a name="creating-resources-with-code"></a>Criando recursos com código  
 Se quiser criar um aplicativo [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] inteiro em código, você também poderá querer criar todos os recursos do aplicativo em código. Para fazer isso, crie uma nova instância de <xref:System.Windows.ResourceDictionary> e, em seguida, adicione todos os recursos ao dicionário usando chamadas sucessivas para <xref:System.Windows.ResourceDictionary.Add%2A?displayProperty=nameWithType>. Em seguida, use o <xref:System.Windows.ResourceDictionary> criado para definir a propriedade <xref:System.Windows.FrameworkElement.Resources%2A> em um elemento que está presente em um escopo de página ou na <xref:System.Windows.Application.Resources%2A?displayProperty=nameWithType>. Você também pode manter o <xref:System.Windows.ResourceDictionary> como um objeto autônomo sem adicioná-lo a um elemento. No entanto, se fizer isso, você precisará acessar os recursos dentro pela chave do item, como se ele fosse um dicionário genérico. Uma <xref:System.Windows.ResourceDictionary> que não está anexada a um elemento `Resources` propriedade não existe como parte da árvore de elementos e não tem nenhum escopo em uma sequência de pesquisa que possa ser usada por <xref:System.Windows.FrameworkElement.FindResource%2A> e métodos relacionados.  
  
<a name="objectaskey"></a>   
## <a name="using-objects-as-keys"></a>Usando objetos como chaves  
 A maioria dos usos do recurso definirá a chave de recurso como uma cadeia de caracteres. No entanto, vários recursos de [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] deliberadamente não usam um tipo de cadeia de caracteres para especificar chaves, em vez disso, este parâmetro é um objeto. A capacidade de ter recursos cujas chaves são objetos é usada pelo suporte a estilos e temas de [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]. Os estilos em temas que se tornam o estilo padrão para um controle não estilizado de outra forma são cada um com um chave pelo <xref:System.Type> do controle ao qual devem ser aplicados. Ter como chave um tipo fornece um mecanismo de pesquisa confiável que funciona em instâncias padrão de cada tipo de controle, e o tipo pode ser detectado por reflexão e usado para estilizar classes derivadas mesmo que o tipo derivado não tenha nenhum estilo padrão. Você pode especificar uma chave de <xref:System.Type> para um recurso definido em [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] usando a [extensão de marcação x:Type](../../xaml-services/x-type-markup-extension.md). Extensões semelhantes existem para outros usos de chave diferentes de cadeias de caracteres que dão suporte a recursos [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)], como a [Extensão de marcação ComponentResourceKey](componentresourcekey-markup-extension.md).  
  
## <a name="see-also"></a>Consulte também

- [Recursos XAML](xaml-resources.md)
- [Estilo e modelagem](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
