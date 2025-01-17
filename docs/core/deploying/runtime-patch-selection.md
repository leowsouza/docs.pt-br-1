---
title: Efetuar roll forward de runtime para implantações autocontidas do .NET Core.
description: Saiba mais as alterações de dotnet publish em implantações autossuficientes.
author: KathleenDollard
ms.date: 05/31/2018
ms.custom: seodec18
ms.openlocfilehash: 6a0cdfb34973822c2f40cdb37d4038d3b7ad8e2a
ms.sourcegitcommit: 4f4a32a5c16a75724920fa9627c59985c41e173c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72522093"
---
# <a name="self-contained-deployment-runtime-roll-forward"></a>Roll forward de runtime de implantação autossuficiente

As [implantações de aplicativo autossuficientes](index.md) do .NET Core incluem as bibliotecas e o tempo de execução do .NET Core. A partir do SDK do .NET Core 2.1 (.NET Core 2.1.300), uma implantação de aplicativo autocontida [publica o tempo de execução de patch mais recente no computador](https://github.com/dotnet/designs/pull/36). Por padrão, [`dotnet publish`](../tools/dotnet-publish.md) para uma implantação autossuficiente seleciona a última versão instalada como parte do SDK no computador de publicação. Isso permite que o aplicativo implantado seja executado com correções de segurança (e outras correções) disponíveis durante `publish`. O aplicativo precisa ser publicado novamente para obter um novo patch. Os aplicativos autocontidos são criados pela especificação de `-r <RID>` no comando `dotnet publish`, pela especificação do [RID (identificador de tempo de execução)](../rid-catalog.md) no arquivo de projeto (csproj/vbproj) ou na linha de comando.

## <a name="patch-version-roll-forward-overview"></a>Visão geral do roll forward da versão de patch

[`restore`](../tools/dotnet-restore.md), [`build`](../tools/dotnet-build.md) e [`publish`](../tools/dotnet-publish.md) são comandos `dotnet` que podem ser executados separadamente. A opção de tempo de execução faz parte da operação `restore`, não de `publish` nem de `build`. Se você chamar `publish`, a última versão de patch será escolhida. Se você chamar `publish` com o argumento `--no-restore`, poderá não obter a versão de patch desejada, porque um `restore` anterior pode não ter sido executado com a nova política de publicação do aplicativo autossuficiente. Nesse caso, um erro de build é gerado com um texto semelhante ao seguinte:

  "O projeto foi restaurado usando o Microsoft.NETCore.App versão 2.0.0, mas com as configurações atuais, a versão 2.0.6 será usada em seu lugar. Para resolver esse problema, verifique se as mesmas configurações são usadas para restauração e para as operações seguintes, como build ou publicação. Normalmente, esse problema pode ocorrer se a propriedade RuntimeIdentifier é definida durante o build ou a publicação, mas não durante a restauração."

> [!NOTE]
> `restore` e `build` podem ser executados implicitamente como parte de outro comando, como `publish`. Quando executados implicitamente como parte de outro comando, eles são fornecidos com um contexto adicional para que os artefatos corretos sejam produzidos. Quando você executa `publish` com um tempo de execução (por exemplo, `dotnet publish -r linux-x64`), o `restore` implícito restaura os pacotes para o tempo de execução linux-x64. Se você chama `restore` explicitamente, ele não restaura os pacotes de tempo de execução por padrão, porque não tem esse contexto.

## <a name="how-to-avoid-restore-during-publish"></a>Como evitar a restauração durante a publicação

A execução de `restore` como parte da operação `publish` pode ser indesejável para seu cenário. Para evitar `restore` durante `publish` ao criar aplicativos autossuficientes, faça o seguinte:

- Defina a propriedade `RuntimeIdentifiers` como uma lista separada por ponto e vírgula de todos os [RIDs](../rid-catalog.md) a serem publicados.
- Defina a propriedade `TargetLatestRuntimePatch` como `true`.

## <a name="no-restore-argument-with-dotnet-publish-options"></a>Argumento no-restore com opções dotnet publish

Caso deseje criar aplicativos autossuficientes e [aplicativos dependentes de estrutura](index.md) com o mesmo arquivo de projeto e usar o argumento `--no-restore` com `dotnet publish`, escolha um dos seguintes:

1. Prefira o comportamento dependente de estrutura. Se o aplicativo depende de uma estrutura, esse é o comportamento padrão. Se o aplicativo for autossuficiente e puder usar um tempo de execução local 2.1.0 sem patch, defina o `TargetLatestRuntimePatch` como `false` no arquivo de projeto.

2. Prefira o comportamento autossuficiente. Se o aplicativo é autossuficiente, esse é o comportamento padrão. Se o aplicativo depende de uma estrutura e exige o último patch instalado, defina `TargetLatestRuntimePatch` como `true` no arquivo de projeto.

3. Assuma o controle explícito da versão da estrutura de tempo de execução definindo `RuntimeFrameworkVersion` como a versão de patch específica no arquivo de projeto.
