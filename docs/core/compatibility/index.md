---
title: Tipos de alterações significativas – .NET Core
description: Saiba como o .NET Core tenta manter a compatibilidade para desenvolvedores em versões do .NET e que tipo de alteração é considerada uma alteração significativa.
ms.date: 06/10/2019
ms.openlocfilehash: 5624a35a0d71224faf9adc5df2b02a529e650314
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74567711"
---
# <a name="changes-that-affect-compatibility"></a>Alterações que afetam a compatibilidade

Ao longo de sua história, o .NET tentou manter um alto nível de compatibilidade de versão para versão e em todos os tipos de .NET. Isso também ocorre no .NET Core. Embora o .NET Core possa ser considerado uma nova tecnologia independente do .NET Framework, dois fatores principais limitam a capacidade do .NET Core de divergir do .NET Framework:

- Um grande número de desenvolvedores originalmente desenvolveu ou continua a desenvolver aplicativos .NET Framework. Eles esperam um comportamento consistente nas implementações do .NET.

- Os projetos da biblioteca .NET Standard permitem que os desenvolvedores criem bibliotecas voltadas a APIs comuns compartilhadas pelo .NET Core e pelo .NET Framework. Os desenvolvedores esperam que uma biblioteca usada em um aplicativo .NET Core se comporte de maneira idêntica à mesma biblioteca usada em um aplicativo .NET Framework.

Além da compatibilidade entre as implementações do .NET, os desenvolvedores esperam um alto nível de compatibilidade nas versões do .NET Core. Em particular, o código escrito para uma versão anterior do .NET Core precisam ser executado sem problemas em uma versão posterior do .NET Core. Na verdade, muitos desenvolvedores esperam que as novas APIs encontradas em versões recém-lançadas do .NET Core também sejam compatíveis com as versões de pré-lançamento em que essas APIs foram apresentadas.

Este artigo descreve as categorias de alterações de compatibilidade (ou alterações significativas) e a maneira como a equipe do .NET avalia alterações em cada uma dessas categorias. Entender como a equipe .NET se aproxima das possíveis alterações significativas é particularmente útil para os desenvolvedores que abrem solicitações pull no repositório do GitHub [dotnet/corefx](https://github.com/dotnet/corefx) que modificam o comportamento das APIs existentes.

> [!NOTE]
> Para obter uma definição das categorias de compatibilidade, como compatibilidade binária e compatibilidade com versões anteriores, confira [Categorias de alterações significativas](categories.md).

As seções a seguir descrevem as categorias de alterações feitas nas APIs do .NET Core e o impacto delas na compatibilidade do aplicativo. O ícone de ✔️ indica que um determinado tipo de alteração é permitido, ❌ indica que ele não é permitido e ❓ indica uma alteração que pode ou não ser permitida. As alterações nessa última categoria exigem Judgement e uma avaliação do quão previsível, óbvio e consistente o comportamento anterior era.

> [!NOTE]
> Além de servir como um guia de como as alterações nas bibliotecas .NET Core são avaliadas, os desenvolvedores de bibliotecas também podem usar esses critérios para avaliar as alterações em suas bibliotecas voltadas a várias implementações e versões do .NET.

## <a name="modifications-to-the-public-contract"></a>Modificações no contrato público

As alterações nessa categoria modificam a área de superfície pública de um tipo. A maioria das alterações nesta categoria não é permitida porque viola a *compatibilidade com versões anteriores* (a capacidade de um aplicativo desenvolvido com a versão anterior de uma API ser executado sem recompilação em uma versão posterior).

### <a name="types"></a>Tipos

- **✔️ Remover uma implementação de interface de um tipo quando a interface já foi implementada por um tipo base**

- **❓ Adicionar uma nova implementação de interface a um tipo**

  Essa é uma alteração aceitável, pois não afeta negativamente os clientes existentes. Quaisquer alterações no tipo precisam funcionar dentro dos limites das alterações aceitáveis ​​definidas aqui para que a nova implementação permaneça aceitável. É necessário ter um extremo cuidado ao adicionar interfaces que afetam diretamente a capacidade de um designer ou serializador de gerar código ou dados que não podem ser consumidos em um nível anterior. Um exemplo é a interface <xref:System.Runtime.Serialization.ISerializable>.

- **❓ Introduzir uma nova classe base**

  Um tipo poderá ser introduzido em uma hierarquia entre dois tipos existentes se ele não introduzir novos membros [abstratos](../../csharp/language-reference/keywords/abstract.md)ou alterar a semântica ou o comportamento de tipos existentes. Por exemplo, no .NET Framework 2.0, a classe <xref:System.Data.Common.DbConnection> tornou-se uma nova classe base para <xref:System.Data.SqlClient.SqlConnection>, que antes era derivada diretamente de <xref:System.ComponentModel.Component>.

- **✔️ Mover um tipo de um assembly para outro**

  O assembly *antigo* precisa ser marcado com o <xref:System.Runtime.CompilerServices.TypeForwardedToAttribute> que aponta para o novo assembly.

- **✔️ Alterar um tipo de [struct](../../csharp/language-reference/keywords/struct.md) para um tipo `readonly struct`**

  A alteração de um tipo `readonly struct` para um tipo `struct` não é permitida.

- **✔️ Adicionar a palavra-chave [sealed](../../csharp/language-reference/keywords/sealed.md) ou [abstract](../../csharp/language-reference/keywords/abstract.md) a um tipo quando não há construtores *accessíveis* (públicos ou protegidos)**

- **✔️ Expandir a visibilidade de um tipo**

- **❌ alterar o namespace ou o nome de um tipo**

- **❌ renomear ou remover um tipo público**

   Isso interrompe todo o código que usa o tipo renomeado ou removido.

- **❌ alterar o tipo subjacente de uma enumeração**

   Esta é uma alteração significativa de tempo de compilação e comportamental, bem como uma alteração significativa binária que pode tornar os argumentos de atributo inseparáveis.

- **❌ selando um tipo que não foi lacrado anteriormente**

- **❌ adicionar uma interface ao conjunto de tipos de base de uma interface**

   Se uma interface implementar uma interface não implementada anteriormente, todos os tipos que implementaram a versão original da interface serão corrompidos.

- **❓ Remover uma classe do conjunto de classes base ou uma interface do conjunto de interfaces implementadas**

  Há uma exceção à regra para remoção de interface: é possível adicionar a implementação de uma interface derivada da interface removida. Por exemplo, você pode remover <xref:System.IDisposable> se o tipo ou interface agora implementa <xref:System.ComponentModel.IComponent>, que implementa <xref:System.IDisposable>.

- **❌ alterar um tipo de `readonly struct` para um tipo de [struct](../../csharp/language-reference/keywords/struct.md)**

  A alteração de um tipo `struct` para um tipo `readonly struct` é permitida.

- **❌ alterar um tipo de [struct](../../csharp/language-reference/keywords/struct.md) para um tipo `ref struct`, e vice-versa**

- **❌ reduzir a visibilidade de um tipo**

   No entanto, aumentar a visibilidade de um tipo é permitido.

### <a name="members"></a>Membros

- **✔️ Expandir a visibilidade de um membro que não é [virtual](../../csharp/language-reference/keywords/sealed.md)**

- **✔️ Adicionar um membro abstrato a um tipo público que não tenha construtores *acessíveis* (públicos ou protegidos) ou a um tipo que seja [sealed](../../csharp/language-reference/keywords/sealed.md)**

  No entanto, adicionar um membro abstrato a um tipo que tenha construtores acessíveis (públicos ou protegidos) e não seja `sealed` não é permitido.

- **✔️ Restringir a visibilidade de um membro [protegido](../../csharp/language-reference/keywords/protected.md) quando o tipo não tem construtores acessíveis (públicos ou protegidos) ou o tipo é [sealed](../../csharp/language-reference/keywords/sealed.md)**

- **✔️ Mover um membro para uma classe superior ao tipo do qual ele foi removido na hierarquia**

- **✔️ Adicionar ou remover uma substituição**

  A introdução de uma substituição pode fazer com que os consumidores anteriores pulem a substituição ao chamar a [base](../../csharp/language-reference/keywords/base.md).

- **✔️ Adicionar um construtor a uma classe, juntamente com um construtor sem parâmetros, se a classe anteriormente não tivesse construtores**

   No entanto, não é permitido adicionar um construtor a uma classe que anteriormente não tinha construtores *sem* adicionar o construtor sem parâmetros.

- **✔️ Alterar um membro de [abstract](../../csharp/language-reference/keywords/abstract.md) para [virtual](../../csharp/language-reference/keywords/virtual.md)**

- **✔️ Alterar de `ref readonly` para um valor retornado `ref` (exceto para interfaces ou métodos virtuais)**

- **✔️ Remover [readonly](../../csharp/language-reference/keywords/readonly.md) de um campo, a menos que o tipo estático do campo seja um tipo de valor mutável**

- **✔️ Chamar um novo evento que não foi definido anteriormente**

- **❓ Adicionar um novo campo de instância a um tipo**

   Essa alteração afeta a serialização.

- **❌ renomear ou remover um membro público ou parâmetro**

   Isso interrompe todo o código que usa o membro ou parâmetro renomeado ou removido.

   Isso inclui remover ou renomear um getter ou setter de uma propriedade, bem como renomear ou remover membros de enumeração.

- **❌ adicionar um membro a uma interface**

- **❌ alterar o valor de uma constante pública ou membro de enumeração**

- **❌ alterar o tipo de uma propriedade, campo, parâmetro ou valor de retorno**

- **❌ adicionar, remover ou alterar a ordem dos parâmetros**

- **❌ adicionar ou remover a palavra-chave [in](../../csharp/language-reference/keywords/in.md), [out](../../csharp/language-reference/keywords/out.md) ou [ref](../../csharp/language-reference/keywords/ref.md) de um parâmetro**

- **❌ renomear um parâmetro (incluindo a alteração de seu caso)**

  Isso é considerado significativo por dois motivos:

  - Interrompe cenários de associação tardia, como o recurso de associação tardia no Visual Basic e [dinâmico](../../csharp/language-reference/builtin-types/reference-types.md#the-dynamic-type) no C#.

  - Interrompe a [compatibilidade de origem](categories.md#source-compatibility) quando os desenvolvedores usam [argumentos nomeados](../../csharp/programming-guide/classes-and-structs/named-and-optional-arguments.md#named-arguments).

- **❌ alterar de um valor de retorno de `ref` para um valor de retorno `ref readonly`**

- **❌️ alteração de um `ref readonly` para um valor de retorno de `ref` em um método ou interface virtual**

- **❌ adicionar ou remover [abstrato](../../csharp/language-reference/keywords/abstract.md) de um membro**

- **❌ remover a palavra-chave [virtual](../../csharp/language-reference/keywords/virtual.md) de um membro**

  Embora isso geralmente não seja uma alteração significativa, pois o compilador C# tende a emitir instruções [callvirt](<xref:System.Reflection.Emit.OpCodes.Callvirt>) de IL (Intermediate Language) para chamar métodos não virtuais (`callvirt` executa uma verificação nula, enquanto uma chamada normal não), esse comportamento não é invariável por vários motivos:
  - C# não é a única linguagem de destino do .NET.

  - O compilador C# tenta otimizar cada vez mais `callvirt` a uma chamada normal sempre que o método de destino é não virtual e provavelmente não é nulo (como um método acessado por meio de operador de propagação nula [?.](../../csharp/language-reference/operators/member-access-operators.md#null-conditional-operators--and-)).

  Tornar um método virtual significa que o código do consumidor em geral acabaria chamando-o não virtualmente.

- **❌ adicionar a palavra-chave [virtual](../../csharp/language-reference/keywords/virtual.md) a um membro**

- **❌ tornar um membro Virtual abstrato**

  Um [membro virtual](../../csharp/language-reference/keywords/virtual.md) fornece uma implementação de método que *pode ser* substituída por uma classe derivada. Um [membro abstrato](../../csharp/language-reference/keywords/abstract.md) não fornece nenhuma implementação e *precisa ser* substituído.

- **❌ adicionar um membro abstrato a um tipo público que tem construtores acessíveis (públicos ou protegidos) e que não está [lacrado](../../csharp/language-reference/keywords/sealed.md)**

- **❌ adicionar ou remover a palavra-chave [estática](../../csharp/language-reference/keywords/static.md) de um membro**

- **❌ adicionar uma sobrecarga que impede uma sobrecarga existente e define um comportamento diferente**

  Isso interrompe os clientes existentes que estavam vinculados à sobrecarga anterior. Por exemplo, se uma classe tiver uma única versão de um método que aceite um <xref:System.UInt32>, um consumidor existente será vinculado a essa sobrecarga ao passar um valor <xref:System.Int32>. No entanto, se você adicionar uma sobrecarga que aceita um <xref:System.Int32>, ao recompilar ou usar associação tardia, o compilador agora se associa à nova sobrecarga. Se resultar em um comportamento diferente, significa que essa é uma alteração significativa.

- **❌ adicionar um construtor a uma classe que anteriormente não tinha nenhum construtor sem adicionar o construtor de parâmetros**

- **❌️ adicionar [ReadOnly](../../csharp/language-reference/keywords/readonly.md) a um campo**

- **❌ reduzir a visibilidade de um membro**

   Isso inclui restringir a visibilidade de um membro [protegido](../../csharp/language-reference/keywords/protected.md) quando o tipo não tem construtores *acessíveis* (públicos ou protegidos) e o tipo *não* é [sealed](../../csharp/language-reference/keywords/sealed.md). Se esse não for o caso, será permitido reduzir a visibilidade de um membro protegido.

   Aumentar a visibilidade de um tipo é permitido.

- **❌ alterar o tipo de um membro**

   Não é possível modificar o valor de retorno de um método ou o tipo de uma propriedade ou campo. Por exemplo, a assinatura de um método que retorna um <xref:System.Object> não pode ser alterada para retornar um <xref:System.String> ou vice-versa.

- **❌ adicionar um campo a uma struct que anteriormente não tinha nenhum estado**

  As regras de atribuição definidas permitem o uso de variáveis ​​não inicializadas, desde que o tipo de variável seja um struct sem estado. Se o struct for alterado para “com estado”, o código poderá acabar com dados não inicializados. Isso é potencialmente uma interrupção de fonte e uma alteração binária significativa.

- **❌ acionar um evento existente quando ele nunca foi acionado antes**

## <a name="behavioral-changes"></a>Alterações comportamentais

### <a name="assemblies"></a>Assemblies

- **✔️ Tornar um assembly portátil quando ainda há suporte para as mesmas plataformas**

- **❌ alterar o nome de um assembly**
- **❌ alterar a chave pública de um assembly**

### <a name="properties-fields-parameters-and-return-values"></a>Propriedades, campos, parâmetros e valores retornados

- **✔️ Alterar o valor de uma propriedade, campo, valor retornado ou parâmetro [out](../../csharp/language-reference/keywords/out-parameter-modifier.md) para um tipo mais derivado**

  Por exemplo, um método que retorna um tipo de <xref:System.Object> pode retornar uma instância <xref:System.String>. (No entanto, a assinatura do método não pode ser alterada.)

- **✔️ Aumentar o intervalo de valores aceitos para uma propriedade ou parâmetro se o membro não for [virtual](../../csharp/language-reference/keywords/virtual.md)**

  Embora o intervalo de valores que podem ser passados ​​para o método ou retornados pelo membro possa ser expandidos, o parâmetro ou o tipo de membro não pode. Por exemplo, enquanto os valores passados ​​para um método podem ser expandidos de 0-124 para 0-255, o tipo de parâmetro não pode ser alterado de <xref:System.Byte> para <xref:System.Int32>.

- **❌ aumentar o intervalo de valores aceitos para uma propriedade ou parâmetro se o membro for [virtual](../../csharp/language-reference/keywords/virtual.md)**

   Essa alteração interrompe os membros substituídos existentes, que não funcionarão corretamente para o intervalo estendido de valores.

- **❌ diminuindo o intervalo de valores aceitos para uma propriedade ou parâmetro**

- **❌ aumentar o intervalo de valores retornados para uma propriedade, campo, valor de retorno ou parâmetro de [saída](../../csharp/language-reference/keywords/out-parameter-modifier.md)**

- **❌ alterar os valores retornados para uma propriedade, campo, valor de retorno de método ou parâmetro de [saída](../../csharp/language-reference/keywords/out-parameter-modifier.md)**

- **❌ alterar o valor padrão de uma propriedade, campo ou parâmetro**

- **❌ alterar a precisão de um valor numérico de retorno**

- **❓ Uma alteração na análise de entrada e no lançamento de novas exceções (mesmo se o comportamento de análise não for especificado na documentação**

### <a name="exceptions"></a>Exceções

- **✔️ Lançar uma exceção mais derivada do que uma exceção existente**

  Como a nova exceção é uma subclasse de uma exceção existente, o código de manipulação de exceção anterior continua a manipular a exceção. Por exemplo, no .NET Framework 4, os métodos de criação e recuperação de cultura começariam a lançar um <xref:System.Globalization.CultureNotFoundException> em vez de um <xref:System.ArgumentException> se a cultura não pudesse ser encontrada. Como <xref:System.Globalization.CultureNotFoundException> deriva de <xref:System.ArgumentException>, essa é uma alteração aceitável.

- **✔️ Lançar uma exceção mais específica do que <xref:System.NotSupportedException>, <xref:System.NotImplementedException>, <xref:System.NullReferenceException>**

- **✔️ Lançar uma exceção que é considerada irrecuperável**

  Exceções irrecuperáveis ​​não devem ser capturadas, mas precisam ser manipuladas por um manipulador de nível alto. Portanto, não se espera que os usuários tenham código que capture essas exceções explícitas. As exceções irrecuperáveis são:

  - <xref:System.AccessViolationException>
  - <xref:System.ExecutionEngineException>
  - <xref:System.Runtime.InteropServices.SEHException>
  - <xref:System.StackOverflowException>

- **✔️ Lançar uma nova exceção em um novo caminho de código**

  A exceção precisa se aplicar apenas a um novo caminho de código que é executado com novos valores de parâmetro ou estado e que não pode ser executado pelo código existente que tem como destino a versão anterior.

- **✔️ Remover uma exceção para permitir um comportamento mais robusto ou novos cenários**

  Por exemplo, um método `Divide` que anteriormente apenas manipulou valores positivos e lançou um <xref:System.ArgumentOutOfRangeException> pode ser alterado para dar suporte a valores negativos e positivos sem gerar uma exceção.

- **✔️ Alterar o texto de uma mensagem de erro**

  Os desenvolvedores não devem se basear no texto de mensagens de erro, que também mudam com base na cultura do usuário.

- **❌ lançar uma exceção em qualquer outro caso não listado acima**

- **❌ remover uma exceção em qualquer outro caso não listado acima**

### <a name="attributes"></a>{1&gt;{2&gt;Atributos&lt;2}&lt;1}

- **✔️ Alterar o valor de um atributo que é *não* observável**

- **❌ alterar o valor de um atributo que *é* observável**

- **❓ Remover um atributo**

  Na maioria dos casos, a remoção de um atributo (como <xref:System.NonSerializedAttribute>) é uma alteração significativa.

## <a name="platform-support"></a>Suporte de plataforma

- **✔️ Dar suporte a uma operação em uma plataforma que não tinha suporte anteriormente**

- **❌ não dá suporte ou agora exigindo uma service pack específica para uma operação que tinha suporte anteriormente em uma plataforma**

## <a name="internal-implementation-changes"></a>Alterações na implementação interna

- **❓ Alterar a área de superfície de um tipo interno**

   Tais mudanças geralmente são permitidas, embora interrompam a reflexão privada. Em alguns casos, em que bibliotecas populares de terceiros ou um grande número de desenvolvedores dependem das APIs internas, essas alterações podem não ser permitidas.

- **❓ Alterar a implementação interna de um membro**

  Essas mudanças geralmente são permitidas, embora interrompam a reflexão privada. Em alguns casos, em que o código do cliente frequentemente depende da reflexão privada ou em que a alteração introduz efeitos colaterais indesejados, essas alterações podem não ser permitidas.

- **✔️ Melhorar o desempenho de uma operação**

   A capacidade de modificar o desempenho de uma operação é essencial, mas essas alterações podem quebrar o código que depende da velocidade atual de uma operação. Isso é particularmente verdadeiro no código que depende do tempo de operações assíncronas. A alteração de desempenho não deve afetar outros comportamentos da API em questão; caso contrário, a alteração será interrompida.

- **✔️ Alterar indiretamente (e, muitas vezes, adversamente) o desempenho de uma operação**

  Se a alteração em questão não for categorizada como significativa por algum outro motivo, isso será aceitável. Geralmente, ações precisam ser realizadas, o que pode incluir operações extras ou a adição de novas funcionalidades. Isso quase sempre afetará o desempenho, mas pode ser essencial para que a API em questão funcione conforme o esperado.

- **❌ alterar uma API síncrona para assíncrona (e vice-versa)**

## <a name="code-changes"></a>Alterações de código

- **✔️ Adicionar [parâmetros](../../csharp/language-reference/keywords/params.md) a um parâmetro**

- **❌ alterar uma [struct](../../csharp/language-reference/keywords/struct.md) para uma [classe](../../csharp/language-reference/keywords/class.md) e vice-versa**

- **❌ adicionar a palavra-chave [Checked](../../csharp/language-reference/keywords/virtual.md) a um bloco de código**

   Essa alteração pode fazer com que um código que foi executado anteriormente lance um <xref:System.OverflowException>, o que é inaceitável.

- **❌ remover [params](../../csharp/language-reference/keywords/params.md) de um parâmetro**

- **❌ alterar a ordem na qual os eventos são acionados**

  Os desenvolvedores podem razoavelmente esperar que os eventos sejam disparados na mesma ordem, e o código do desenvolvedor frequentemente depende da ordem em que os eventos são disparados.

- **❌ remover a geração de um evento em uma determinada ação**

- **❌ alterar o número de vezes que os eventos fornecidos são chamados**

- **❌ adicionar o <xref:System.FlagsAttribute> a um tipo de enumeração**
