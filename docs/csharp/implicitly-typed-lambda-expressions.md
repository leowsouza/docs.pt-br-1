---
title: Expressões lambda tipadas implicitamente
description: Saiba porque você não pode usar uma declaração de variável tipada implicitamente para declarar uma expressão lambda.
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: a3851da9-e018-4389-9922-233db7d0f841
ms.openlocfilehash: c6b0f2666a5c67ce8c89222da5959304ecb8fb93
ms.sourcegitcommit: ad800f019ac976cb669e635fb0ea49db740e6890
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73039135"
---
# <a name="implicitly-typed-lambda-expressions"></a>Expressões lambda tipadas implicitamente

Você não pode usar uma declaração de variável tipada implicitamente para declarar uma expressão lambda.
Isso cria um problema de lógica circular para o compilador. A declaração `var` diz ao compilador para descobrir o tipo da variável de acordo com o tipo da expressão no lado direito do operador de atribuição. Uma expressão lambda não tem um tipo de tempo de compilação, mas pode ser convertida em qualquer tipo de delegado ou expressão correspondente. Quando atribui uma expressão lambda a uma variável de um tipo de delegado ou expressão, você diz ao compilador para tentar converter a expressão lambda em uma expressão ou delegado que corresponda à assinatura da variável "atribuída". O compilador deve tentar fazer o item do lado direito da atribuição corresponder ao tipo do lado esquerdo da atribuição. 

Os dois lados da atribuição não podem estar dizendo ao compilador para examinar o objeto do outro lado do operador de atribuição para ver se meu tipo é correspondente.

Você pode obter mais detalhes sobre por quê a linguagem C# especifica esse comportamento lendo [este artigo](https://download.microsoft.com/download/5/4/B/54B83DFE-D7AA-4155-9687-B0CF58FF65D7/type-inference.pdf) (download do PDF)
