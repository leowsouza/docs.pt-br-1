---
title: Instrução using – Referência de C#
ms.custom: seodec18
ms.date: 10/15/2019
helpviewer_keywords:
- using statement [C#]
ms.assetid: afc355e6-f0b9-4240-94dd-0d93f17d9fc3
ms.openlocfilehash: f5ff78eaf9d565a9708c7a3a11754579389e79e8
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2019
ms.locfileid: "73422240"
---
# <a name="using-statement-c-reference"></a>Instrução using (Referência de C#)

Fornece uma sintaxe conveniente que garante o uso correto de objetos <xref:System.IDisposable>.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como usar a instrução `using`.

[!code-csharp[csrefKeywordsNamespace#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#4)]

A partir C# do 8,0, você pode usar a seguinte sintaxe alternativa para a instrução de `using` que não exige chaves:

[!code-csharp[csrefKeywordsNamespace#New](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#ModernUsing)]

## <a name="remarks"></a>Comentários

<xref:System.IO.File> e <xref:System.Drawing.Font> são exemplos de tipos gerenciados que acessam recursos não gerenciados (nesse caso, identificadores de arquivo de caso e contextos de dispositivo). Há muitos outros tipos de recursos não gerenciados e tipos de biblioteca de classes que os encapsula. Todos esses tipos devem implementar a interface <xref:System.IDisposable>.

Quando o tempo de vida de um objeto `IDisposable` é limitado a um único método, você deve declará-lo e instanciá-lo na instrução `using`. A instrução `using` chama o método <xref:System.IDisposable.Dispose%2A> no objeto da forma correta e (quando você o usa como mostrado anteriormente) ele também faz com que o objeto em si saia do escopo assim que <xref:System.IDisposable.Dispose%2A> é chamado. Dentro do bloco `using`, o objeto é somente leitura e não pode ser modificado ou reatribuído.

A instrução `using` garante que <xref:System.IDisposable.Dispose%2A> seja chamado, mesmo que ocorra uma exceção dentro do bloco `using`. Você pode obter o mesmo resultado colocando o objeto dentro de um bloco `try` e então chamando <xref:System.IDisposable.Dispose%2A> em um bloco `finally`. Na verdade, é dessa forma que a instrução `using` é convertida pelo compilador. O exemplo de código anterior se expande para o seguinte código em tempo de compilação (observe as chaves extras para criar o escopo limitado para o objeto):

[!code-csharp[csrefKeywordsNamespace#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#5)]

A sintaxe de instrução `using` mais recente se traduz em um código muito semelhante. O bloco de `try` é aberto onde a variável é declarada. O bloco de `finally` é adicionado ao fechamento do bloco delimitador, normalmente no final de um método.

Para obter mais informações sobre a instrução `try`-`finally`, veja o tópico [try-finally](try-finally.md).

Várias instâncias de um tipo podem ser declaradas na instrução `using`, conforme mostra o exemplo a seguir:

[!code-csharp[csrefKeywordsNamespace#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#6)]

Você pode combinar várias declarações do mesmo tipo usando a nova sintaxe introduzida com C# 8 também. Isso é mostrado no exemplo a seguir:

[!code-csharp[csrefKeywordsNamespace#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#MultipleUsing)]

Você pode criar uma instância do objeto de recurso e, em seguida, passar a variável para a instrução `using`, mas isso não é uma prática recomendada. Nesse caso, após o controle sair do bloco `using`, o objeto permanecerá no escopo, mas provavelmente não terá acesso a seus recursos não gerenciados. Em outras palavras, ele não é mais totalmente inicializado. Se você tentar usar o objeto fora do bloco `using`, corre o risco de causar o lançamento de uma exceção. Por esse motivo, geralmente é melhor instanciar o objeto na instrução `using` e limitar seu escopo ao bloco `using`.

[!code-csharp[csrefKeywordsNamespace#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#7)]

Para obter mais informações sobre como descartar objetos `IDisposable`, veja [Usando objetos que implementam IDisposable](../../../standard/garbage-collection/using-objects.md).

## <a name="c-language-specification"></a>Especificação da linguagem C#

Para obter mais informações, consulte [A instrução using](~/_csharplang/spec/statements.md#the-using-statement) na [Especificação da linguagem C#](/dotnet/csharp/language-reference/language-specification/introduction). A especificação da linguagem é a fonte definitiva para a sintaxe e o uso de C#.

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Guia de Programação em C#](../../programming-guide/index.md)
- [Palavras-chave do C#](index.md)
- [Diretiva using](using-directive.md)
- [Coleta de lixo](../../../standard/garbage-collection/index.md)
- [Usando objetos que implementam IDisposable](../../../standard/garbage-collection/using-objects.md)
- [Interface IDisposable](xref:System.IDisposable)
- [usando a instrução C# em 8,0](~/_csharplang/proposals/csharp-8.0/using.md)
