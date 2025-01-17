---
title: Como forçar um argumento a ser passado por Valor
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedures [Visual Basic], parameters
- procedure arguments
- Visual Basic code, procedures
- arguments [Visual Basic], ByVal
- arguments [Visual Basic], passing by value
- procedure parameters
- procedures [Visual Basic], calling
- arguments [Visual Basic], in parentheses
- procedure arguments [Visual Basic], in parentheses
- arguments [Visual Basic], changing value
ms.assetid: 77b4f2d2-1055-4c2f-a521-874d1db86946
ms.openlocfilehash: 8261d126f988bdcf05b4a2af3106b38717e46bc8
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344524"
---
# <a name="how-to-force-an-argument-to-be-passed-by-value-visual-basic"></a>Como forçar um argumento a ser passado por valor (Visual Basic)
A declaração de procedimento determina o mecanismo de passagem. Se um parâmetro for declarado como [ByRef](../../../../visual-basic/language-reference/modifiers/byref.md), Visual Basic esperará passar o argumento correspondente por referência. Isso permite que o procedimento altere o valor do elemento de programação subjacente ao argumento no código de chamada. Se você deseja proteger o elemento subjacente contra essa alteração, você pode substituir o `ByRef` mecanismo de passagem na chamada de procedimento ao colocar o nome do argumento entre parênteses. Esses parênteses são além dos parênteses que envolvem a lista de argumentos na chamada.  
  
 O código de chamada não pode substituir um mecanismo [ByVal](../../../../visual-basic/language-reference/modifiers/byval.md) .  
  
### <a name="to-force-an-argument-to-be-passed-by-value"></a>Para forçar um argumento a ser passado por valor  
  
- Se o parâmetro correspondente for declarado `ByVal` no procedimento, você não precisará executar etapas adicionais. Visual Basic já espera passar o argumento por valor.  
  
- Se o parâmetro correspondente for declarado `ByRef` no procedimento, coloque o argumento entre parênteses na chamada de procedimento.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir substitui uma declaração de parâmetro `ByRef`. Na chamada que força `ByVal`, observe os dois níveis de parênteses.  
  
 [!code-vb[VbVbcnProcedures#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#39)]  
  
 [!code-vb[VbVbcnProcedures#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#40)]  
  
 Quando `str` é colocado entre parênteses extras na lista de argumentos, o procedimento de `setNewString` não pode alterar seu valor no código de chamada e `MsgBox` exibe "não pode ser substituído se passado por ByVal". Quando `str` não está entre parênteses extras, o procedimento pode alterá-lo e `MsgBox` exibe "Este é um novo valor para o argumento inString".  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Ao passar uma variável por referência, você deve usar a palavra-chave `ByRef` para especificar esse mecanismo.  
  
 O padrão no Visual Basic é passar argumentos por valor. No entanto, é uma boa prática de programação incluir a palavra-chave [ByVal](../../../../visual-basic/language-reference/modifiers/byval.md) ou [ByRef](../../../../visual-basic/language-reference/modifiers/byref.md) com cada parâmetro declarado. Isso torna seu código mais fácil de ler.  
  
## <a name="robust-programming"></a>Programação robusta  
 Se um procedimento declarar um parâmetro [ByRef](../../../../visual-basic/language-reference/modifiers/byref.md), a execução correta do código poderá depender da possibilidade de alterar o elemento subjacente no código de chamada. Se o código de chamada substituir esse mecanismo de chamada ao colocar o argumento entre parênteses ou se ele passar um argumento não modificável, o procedimento não poderá alterar o elemento subjacente. Isso pode produzir resultados inesperados no código de chamada.  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 Sempre há um risco potencial em permitir que um procedimento altere o valor subjacente a um argumento no código de chamada. Certifique-se de que você espera que esse valor seja alterado e esteja preparado para verificá-lo quanto à validade antes de usá-lo.  
  
## <a name="see-also"></a>Consulte também

- [Procedimentos](./index.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Como passar argumentos para um procedimento](./how-to-pass-arguments-to-a-procedure.md)
- [Passando Argumentos por Valor e por Referência](./passing-arguments-by-value-and-by-reference.md)
- [Diferenças entre argumentos modificáveis e não modificáveis](./differences-between-modifiable-and-nonmodifiable-arguments.md)
- [Diferenças entre passar um argumento por valor e por referência](./differences-between-passing-an-argument-by-value-and-by-reference.md)
- [Como alterar o valor de um argumento de procedimento](./how-to-change-the-value-of-a-procedure-argument.md)
- [Como proteger um argumento de procedimento contra alterações de valor](./how-to-protect-a-procedure-argument-against-value-changes.md)
- [Passando Argumentos por Posição e Nome](./passing-arguments-by-position-and-by-name.md)
- [Tipos de Valor e Tipos de Referência](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
