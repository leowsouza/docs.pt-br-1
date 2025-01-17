---
title: Geração e compilação de código-fonte dinâmico
ms.date: 03/30/2017
helpviewer_keywords:
- Code Document Object Model
- System.CodeDom namespace
- language-independent source code modeling
- CodeDOM
- multiple languages supported by CodeDOM
- source code in multiple languages
- languages, multiple language support by CodeDOM
ms.assetid: d077a3e8-bd81-4bdf-b6a3-323857ea30fb
ms.openlocfilehash: a7e341bb5bfb5b4648a222409951275169a29b79
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130248"
---
# <a name="dynamic-source-code-generation-and-compilation"></a>Geração e compilação de código-fonte dinâmico
O .NET Framework inclui um mecanismo chamado CodeDOM (Modelo de Objeto do Documento de Código) que permite aos desenvolvedores de programas que emitem o código-fonte gerar o código-fonte em várias linguagens de programação no tempo de execução com base em um único modelo que representa o código a ser renderizado.  
  
 Para representar o código-fonte, os elementos do CodeDOM são vinculados uns aos outros para formar uma estrutura de dados conhecida como um grafo CodeDOM, que modela a estrutura de parte do código-fonte.  
  
 O namespace `System.CodeDom` define tipos que podem representam a estrutura lógica do código-fonte, independente de uma linguagem de programação específica. O namespace `System.CodeDom.Compiler` define tipos para gerar código-fonte de grafos CodeDOM e gerenciar a compilação do código-fonte em linguagens com suporte. Fornecedores de compilador ou desenvolvedores podem estender o conjunto de idiomas com suporte.  
  
 A modelagem de código-fonte independente de linguagem pode ser útil quando um programa precisa gerar código-fonte para um modelo de programa em várias linguagens ou para uma linguagem de destino incerta. Por exemplo, alguns designers usam CodeDOM como uma interface da abstração de linguagem para produzir código-fonte na linguagem de programação correta, caso haja suporte para a linguagem no CodeDOM.  
  
 O .NET Framework inclui geradores e compiladores de código para <xref:Microsoft.CSharp.CSharpCodeProvider>, <xref:Microsoft.JScript.JScriptCodeProvider> e <xref:Microsoft.VisualBasic.VBCodeProvider>.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Usando o CodeDOM](using-the-codedom.md)  
 Descreve os usos comuns e demonstra como criar um grafo de objeto simples usando o CodeDOM.  
  
 [Gerando e código-fonte e compilando um programa de um gráfico CodeDOM](generating-and-compiling-source-code-from-a-codedom-graph.md)  
 Descreve como gerar o código-fonte e compilar o código gerado com um compilador externo usando classes definidas no namespace `System.CodeDom.Compiler`.  
  
 [Como criar um arquivo de documentação XML usando CodeDOM](how-to-create-an-xml-documentation-file-using-codedom.md)  
 Descreve como usar CodeDOM para gerar código com comentários de documentação XML e compilar o código gerado para que ele cria a saída de documentação XML.  
  
 [Como criar uma classe usando o CodeDOM](how-to-create-a-class-using-codedom.md)  
 Descreve como usar CodeDOM para gerar uma classe que contém campos, propriedades, um método, um construtor e um ponto de entrada.  
  
## <a name="reference"></a>Referência  
 <xref:System.CodeDom>  
 Define os elementos que representam os elementos de código em linguagens de programação que usam o Common Language Runtime como destino.  
  
 <xref:System.CodeDom.Compiler>  
 Define as interfaces para gerar e compilar o código no tempo de execução.  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Referência rápida do CodeDOM](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/f1dfsbhc(v=vs.100))  
 Fornece uma maneira rápida para os desenvolvedores localizarem os elementos do CodeDOM que representam os elementos de código-fonte.
