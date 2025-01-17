---
title: Introdução ao .NET Framework
ms.custom: updateeachrelease
ms.date: 04/02/2019
helpviewer_keywords:
- .NET Framework, getting started
- getting started [.NET Framework]
ms.assetid: c693fd34-88fe-4d90-b332-19eeadf3b7e7
ms.openlocfilehash: 563416f1ca9ddb9cb9bb7e78b9e13406983bdf9f
ms.sourcegitcommit: 93762e1a0dae1b5f64d82eebb7b705a6d566d839
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74552797"
---
# <a name="get-started-with-the-net-framework"></a>Introdução ao .NET Framework

O .NET Framework é um ambiente de execução do runtime que gerencia os aplicativos que direcionam o .NET Framework. Ele consiste no Common Language Runtime, que fornece gerenciamento de memória e outros serviços do sistema, além de em uma biblioteca de classes extensa, o que permite que programadores usem o código robusto e confiável para todas as áreas principais do desenvolvimento de aplicativos.

> [!NOTE] 
> O .NET Framework está disponível somente em sistemas Windows. Você pode usar o [.NET Core](../../core/index.md) para executar aplicativos no Windows, MacOS e Linux. 

## <a name="Introducing"></a> O que é o .NET Framework?

O .NET Framework é um ambiente de execução gerenciado para Windows que oferece uma variedade de serviços aos aplicativos em execução. Ele consiste em dois componentes principais: o CLR (Common Language Runtime), o mecanismo de execução que manipula aplicativos em execução, e a biblioteca de classes .NET Framework, que oferece uma biblioteca de códigos testados e reutilizáveis que os desenvolvedores podem chamar de seus próprios aplicativos. Entre os serviços que o .NET Framework fornece aos aplicativos em execução estão os seguintes:

- Gerenciamento de memória. Em muitas linguagens de programação, os programadores são responsáveis por alocar e liberar memória e por identificar o tempo de vida do objeto. Em aplicativos do .NET Framework, o CLR fornece esses serviços em nome do aplicativo.

- Um Common Type System. Em linguagens de programação tradicionais, os tipos básicos são definidos pelo compilador, que complica a interoperabilidade entre linguagens. No .NET Framework, os tipos básicos são definidos pelo sistema de tipos do .NET Framework e são comuns a todas as linguagens que segmentam o .NET Framework.

- Uma biblioteca de classes abrangente. Em vez de precisar gravar grandes volumes de código para manipular operações de programação comuns de baixo nível, os programadores usam uma biblioteca de tipos facilmente acessível e seus membros da biblioteca de classes .NET Framework.

- Estruturas e tecnologias de desenvolvimento. O .NET Framework inclui bibliotecas para áreas específicas do desenvolvimento de aplicativos, como o ASP.NET para aplicativos Web, o ADO.NET para acesso a dados e o Windows Communication Foundation para aplicativos orientados a serviços e Windows Presentation Foundation para aplicativos de área de trabalho Windows.

- Interoperabilidade da linguagem. Compiladores de linguagens que direcionam o .NET Framework emitem um código intermediário chamado de CIL (Common Intermediate Language), que, por sua vez, é compilado no tempo de execução pelo Common Language Runtime. Com esse recurso, as rotinas gravadas em uma linguagem são acessíveis a outras linguagens, e os programadores privilegiam a criação de aplicativos em suas linguagens preferidas.

- Compatibilidade de versões. Com raras exceções, os aplicativos desenvolvidos usando uma versão específica do .NET Framework são executados sem modificação em uma versão posterior.

- Execução lado a lado. O .NET Framework ajuda a resolver conflitos de versão permitindo que várias versões do CLR existam no mesmo computador. Isso significa que várias versões dos aplicativos podem coexistir e que um aplicativo pode ser executado na versão do .NET Framework com a qual foi compilada. A execução lado a lado aplica-se aos grupos de versão do .NET Framework 1.0/1.1, 2.0/3.0/3.5 e 4/4.5.x/4.6.x/4.7.x/4.8.

- Multiplataforma. Ao direcionar o [.NET Standard](../../standard/net-standard.md), desenvolvedores criam bibliotecas de classes que funcionam em várias plataformas do .NET Framework com suporte por essa versão do padrão. Por exemplo, as bibliotecas que direcionam o .NET Standard 2.0 podem ser usadas por aplicativos que direcionam .NET Framework 4.6.1, .NET Core 2.0 e UWP 10.0.16299. 

<a name="ForUsers"></a>
## <a name="the-net-framework-for-users"></a>O .NET Framework para usuários

Se você não desenvolve aplicativos .NET Framework, mas os usa, não é necessário ter nenhum conhecimento específico sobre o .NET Framework ou sobre seu funcionamento. Geralmente, o .NET Framework é totalmente transparente para os usuários.

Se você estiver usando o sistema operacional Windows, o .NET Framework talvez já esteja instalado em seu computador. Além disso, se você instalar um aplicativo que exige o .NET Framework, o programa de instalação do aplicativo poderá instalar uma versão específica do .NET Framework no seu computador. Em alguns casos, você pode ver uma caixa de diálogo solicitando a instalação do .NET Framework. Se você acabou de tentar executar um aplicativo quando essa caixa de diálogo apareceu e se o seu computador tiver acesso à Internet, será possível acessar uma página da Web que permita instalar a versão ausente do .NET Framework. Para obter mais informações, consulte o [Guia de instalação](../install/index.md).

Em geral, você não deve desinstalar versões do .NET Framework instaladas em seu computador. Há dois motivos para isso:

- Se um aplicativo usado depender de uma versão específica do .NET Framework, ele poderá ser interrompido se essa versão for removida.

- Algumas versões do .NET Framework são atualizações in-loco de versões anteriores. Por exemplo, o .NET Framework 3.5 é uma atualização in-loco para a versão 2.0 e o .NET Framework 4.8 é uma atualização in-loco para as versões 4 a 4.7.2. Para obter mais informações, confira [Versões e dependências do .NET Framework](../migration-guide/versions-and-dependencies.md).

Em versões do Windows anteriores ao Windows 8, se você optar por remover o .NET Framework, sempre use **Programas e Recursos** no Painel de Controle para desinstalá-lo. Nunca remova manualmente uma versão do .NET Framework. No Windows 8 e posterior, o .NET Framework é um componente do sistema operacional e não pode ser desinstalado independentemente.

Observe que várias versões do .NET Framework podem coexistir simultaneamente em um único computador. Isso significa que você não precisa desinstalar as versões anteriores para instalar uma versão posterior.

## <a name="ForDevelopers"></a> .NET Framework para desenvolvedores

Se você for um desenvolvedor, escolha qualquer linguagem de programação que dê suporte ao .NET Framework para criar seus aplicativos. Como o .NET Framework oferece independência e interoperabilidade de linguagem, você interage com outros aplicativos e componentes do .NET Framework, independentemente da linguagem com a qual foram desenvolvidos.

Para desenvolver aplicativos ou componentes do .NET Framework, faça o seguinte:

1. Se ele não vier pré-instalado em seu sistema operacional, instale a versão do .NET Framework destinada a seu aplicativo. A versão de produção mais recente é o .NET Framework 4.8. Ele vem pré-instalado na Atualização de maio de 2019 para o Windows 10 e está disponível para download em versões anteriores do sistema operacional Windows. Para conhecer os requisitos de sistema do .NET Framework, confira [Requisitos de sistema](system-requirements.md). Para saber como instalar outras versões do .NET Framework, confira o [Guia de instalação](../install/guide-for-developers.md). Os pacotes adicionais do .NET Framework são lançados fora de banda, o que significa que eles são lançados de forma contínua fora de um ciclo de lançamento regular ou agendado. Para saber mais sobre esses pacotes, confira [O .NET Framework e lançamentos fora da banda](the-net-framework-and-out-of-band-releases.md).

2. Selecione a linguagem ou linguagens com suporte do .NET Framework que você pretende usar para desenvolver seus aplicativos. Há um grande número de linguagens disponível, incluindo [Visual Basic](../../visual-basic/index.md), [C#](../../csharp/index.yml), [F#](../../fsharp/index.yml) e [C++/CLI](/cpp/dotnet/dotnet-programming-with-cpp-cli-visual-cpp) da Microsoft. (Uma linguagem de programação que permite que você desenvolva aplicativos para o .NET Framework adere à [especificação de CLI [Common Language Infrastructure]](https://visualstudio.microsoft.com/license-terms/ecma-c-common-language-infrastructure-standards/).)

3. Para criar seus aplicativos , selecione e instale o ambiente de desenvolvimento a ser usado que dê suporte à linguagem ou às linguagens de programação selecionadas. O IDE (ambiente de desenvolvimento integrado) da Microsoft para aplicativos .NET Framework é o [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link). Está disponível em várias edições.

Para obter mais informações sobre o desenvolvimento de aplicativos direcionados ao .NET Framework, consulte o [Guia de desenvolvimento](../development-guide.md).

## <a name="related-articles"></a>{1&gt;{2&gt;Artigos relacionados&lt;2}&lt;1}

| Cargo | Descrição |
| ----- |------------ |
| [Visão Geral](overview.md) | Fornece informações detalhadas para desenvolvedores que criam aplicativos direcionados ao .NET Framework. |
| [Guia de instalação](../install/index.md) | Fornece informações sobre como instalar o .NET Framework. |  
| [O .NET Framework e lançamentos fora da banda](the-net-framework-and-out-of-band-releases.md) | Descreve os lançamentos fora de banda do .NET Framework e como usá-los em seu aplicativo. |
| [Requisitos do sistema](system-requirements.md) | Lista os requisitos de hardware e software para executar o .NET Framework. |
| [.NET Core e software livre](net-core-and-open-source.md) | Descreve o .NET Core em relação ao .NET Framework e como acessar os projetos de software livre do .NET Core. |
| [Documentação do .NET Core](../../core/index.md) | Fornece a documentação conceitual e de referência de API para .NET Core. |
| [.NET Standard](../../standard/net-standard.md) | Discute .NET padrão, uma especificação de versão que dá suporte a implementações de .NET individuais para garantir que um conjunto consistente de APIs sejam disponíveis em várias plataformas.

## <a name="see-also"></a>Consulte também

- [Guia do .NET Framework](../index.md)
- [Novidades](../whats-new/index.md)
- [Navegador de API do .NET](../../../api/index.md)
- [Guia de desenvolvimento](../development-guide.md)
