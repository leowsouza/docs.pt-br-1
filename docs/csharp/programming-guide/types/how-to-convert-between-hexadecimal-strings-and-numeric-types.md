---
title: Como converter entre cadeias de caracteres hexadecimais e tipos numéricos – guia de C# programação
ms.custom: seodec18
ms.date: 07/20/2015
helpviewer_keywords:
- hexadecimal strings [C#], converting to numeric type
- conversions [C#], hexidecimal strings
- strings [C#], converting hexadecimal strings
- hexadecimal strings [C#]
ms.assetid: 7115c49f-7d1d-40c3-8bd9-aae0cc1d46b6
ms.openlocfilehash: 8b72734f9b617fed2ff65977c9a0e60f46424ae8
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74429445"
---
# <a name="how-to-convert-between-hexadecimal-strings-and-numeric-types-c-programming-guide"></a>Como converter entre cadeias de caracteres hexadecimais e tipos numéricos (Guia de Programação em C#)
Estes exemplos mostram como realizar as seguintes tarefas:  
  
- Obter o valor hexadecimal de cada caractere em uma [cadeia de caracteres](../../language-reference/builtin-types/reference-types.md).  
  
- Obter o [char](../../language-reference/builtin-types/char.md) que corresponde a cada valor em uma cadeia de caracteres hexadecimal.  
  
- Converter um `string` hexadecimal em um [int](../../language-reference/builtin-types/integral-numeric-types.md).  
  
- Converter um `string` hexadecimal em um [float](../../language-reference/builtin-types/floating-point-numeric-types.md).  
  
- Como converter uma matriz de [bytes](../../language-reference/builtin-types/integral-numeric-types.md) em um `string` hexadecimal.  
  
## <a name="example"></a>Exemplo  
 Este exemplo gera o valor hexadecimal de cada caractere em um `string`. Primeiro, ele analisa o `string` como uma matriz de caracteres. Em seguida, ele chama <xref:System.Convert.ToInt32%28System.Char%29> em cada caractere para obter seu valor numérico. Por fim, ele formata o número como sua representação hexadecimal em um `string`.  
  
 [!code-csharp[csProgGuideTypes#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#30)]  
  
## <a name="example"></a>Exemplo  
 Este exemplo analisa um `string` de valores hexadecimais e gera o caractere correspondente a cada valor hexadecimal. Primeiro, ele chama o método [Split(Char\[\])](xref:System.String.Split(System.Char[])) para obter cada valor hexadecimal como um `string` individual em uma matriz. Em seguida, ele chama <xref:System.Convert.ToInt32%28System.String%2CSystem.Int32%29> para converter o valor hexadecimal em um valor decimal representado como um [int](../../language-reference/builtin-types/integral-numeric-types.md). Ele mostra duas maneiras diferentes de obter o caractere correspondente a esse código de caractere. A primeira técnica usa <xref:System.Char.ConvertFromUtf32%28System.Int32%29>, que retorna o caractere correspondente ao argumento de inteiro como um `string`. A segunda técnica converte explicitamente o `int` em um [char](../../language-reference/builtin-types/char.md).  
  
 [!code-csharp[csProgGuideTypes#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#31)]  
  
## <a name="example"></a>Exemplo  
 Este exemplo mostra outra maneira de converter um hexadecimal `string` em um inteiro, chamando o método <xref:System.Int32.Parse%28System.String%2CSystem.Globalization.NumberStyles%29>.  
  
 [!code-csharp[csProgGuideTypes#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#32)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como converter um hexadecimal `string` para um [float](../../language-reference/builtin-types/floating-point-numeric-types.md) usando a classe <xref:System.BitConverter?displayProperty=nameWithType> e o método <xref:System.UInt32.Parse%2A?displayProperty=nameWithType>.  
  
 [!code-csharp[csProgGuideTypes#39](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#39)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como converter uma matriz de [bytes](../../language-reference/builtin-types/integral-numeric-types.md) em uma cadeia de caracteres hexadecimal usando a classe <xref:System.BitConverter?displayProperty=nameWithType>.  
  
 [!code-csharp[csProgGuideTypes#38](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#38)]  
  
## <a name="see-also"></a>Consulte também

- [Cadeias de Caracteres de Formato Numérico Padrão](../../../standard/base-types/standard-numeric-format-strings.md)
- [Tipos](./index.md)
- [Como determinar se uma cadeia de caracteres representa um valor numérico](../strings/how-to-determine-whether-a-string-represents-a-numeric-value.md)
