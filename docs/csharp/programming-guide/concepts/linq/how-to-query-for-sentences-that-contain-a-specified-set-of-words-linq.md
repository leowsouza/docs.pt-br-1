---
title: 'Como: Consultar sentenças que contêm um conjunto especificado de palavras (LINQ) (C#)'
ms.date: 07/20/2015
ms.assetid: 0724b429-4b87-4d26-a7b1-409358f3fc20
ms.openlocfilehash: e478364f07c08a334692ba54ae1a95880b95519e
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69592773"
---
# <a name="how-to-query-for-sentences-that-contain-a-specified-set-of-words-linq-c"></a>Como: Consultar sentenças que contêm um conjunto especificado de palavras (LINQ) (C#)
Este exemplo mostra como localizar frases em um arquivo de texto que contenham correspondências para cada conjunto de palavras especificado. Embora a matriz de termos de pesquisa esteja embutida em código neste exemplo, ela também poderia ser populada dinamicamente em tempo de execução. Neste exemplo, a consulta retorna as frases que contêm as palavras "Historically", "data" e "integrated".  
  
## <a name="example"></a>Exemplo  
  
```csharp  
class FindSentences  
{  
    static void Main()  
    {  
        string text = @"Historically, the world of data and the world of objects " +  
        @"have not been well integrated. Programmers work in C# or Visual Basic " +  
        @"and also in SQL or XQuery. On the one side are concepts such as classes, " +  
        @"objects, fields, inheritance, and .NET Framework APIs. On the other side " +  
        @"are tables, columns, rows, nodes, and separate languages for dealing with " +  
        @"them. Data types often require translation between the two worlds; there are " +  
        @"different standard functions. Because the object world has no notion of query, a " +  
        @"query can only be represented as a string without compile-time type checking or " +  
        @"IntelliSense support in the IDE. Transferring data from SQL tables or XML trees to " +  
        @"objects in memory is often tedious and error-prone.";  
  
        // Split the text block into an array of sentences.  
        string[] sentences = text.Split(new char[] { '.', '?', '!' });  
  
        // Define the search terms. This list could also be dynamically populated at runtime.  
        string[] wordsToMatch = { "Historically", "data", "integrated" };  
  
        // Find sentences that contain all the terms in the wordsToMatch array.  
        // Note that the number of terms to match is not specified at compile time.  
        var sentenceQuery = from sentence in sentences  
                            let w = sentence.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' },  
                                                    StringSplitOptions.RemoveEmptyEntries)  
                            where w.Distinct().Intersect(wordsToMatch).Count() == wordsToMatch.Count()  
                            select sentence;  
  
        // Execute the query. Note that you can explicitly type  
        // the iteration variable here even though sentenceQuery  
        // was implicitly typed.   
        foreach (string str in sentenceQuery)  
        {  
            Console.WriteLine(str);  
        }  
  
        // Keep the console window open in debug mode.  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
}  
/* Output:  
Historically, the world of data and the world of objects have not been well integrated  
*/  
```  
  
 A consulta funciona primeiro dividindo o texto em frases e, em seguida, dividindo as sentenças em uma matriz de cadeias de caracteres que contêm cada palavra. Para cada uma dessas matrizes, o método <xref:System.Linq.Enumerable.Distinct%2A> remove todas as palavras duplicadas e, em seguida, a consulta executa uma operação <xref:System.Linq.Enumerable.Intersect%2A> na matriz de palavras e na matriz `wordsToMatch`. Se a contagem da interseção for igual à contagem da matriz `wordsToMatch`, todas as palavras foram encontradas nas palavras e a frase original será retornada.  
  
 Na chamada para <xref:System.String.Split%2A>, as marcas de pontuação são usadas como separadores para removê-las da cadeia de caracteres. Se não fizer isso, por exemplo, você poderia ter uma cadeia de caracteres "Historically" que não corresponderia a "Historically" na matriz `wordsToMatch`. Talvez você precise usar separadores adicionais, dependendo dos tipos de pontuação encontrados no texto de origem.  
  
## <a name="compiling-the-code"></a>Compilando o código  
Criar um projeto de aplicativo de console em C# com diretivas `using` para os namespaces System.Linq e System.IO.

## <a name="see-also"></a>Consulte também

- [LINQ e cadeias de caracteres (C#)](./linq-and-strings.md)
