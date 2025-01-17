---
title: -baseaddress (opções do compilador C#)
ms.date: 07/20/2015
f1_keywords:
- /dllbase
helpviewer_keywords:
- baseaddress compiler option [C#]
- -baseaddress compiler option [C#]
- /baseaddress compiler option [C#]
ms.assetid: ce13c965-dfe4-4433-94f5-63b476e3a608
ms.openlocfilehash: e96ab3ece6edc36c913a8efc0097ff9c4a1e3c22
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69607035"
---
# <a name="-baseaddress-c-compiler-options"></a>-baseaddress (opções do compilador C#)
A opção **-baseaddress** permite especificar o endereço básico preferido em que uma DLL será carregada. Para obter mais informações sobre quando e por que usar essa opção, consulte o [Blog do Larry Osterman](https://blogs.msdn.microsoft.com/larryosterman/2004/07/06/why-should-i-even-bother-to-use-dlls-in-my-system/).  
  
## <a name="syntax"></a>Sintaxe  
  
```console  
-baseaddress:address  
```  
  
## <a name="arguments"></a>Arguments  
 `address`  
 O endereço básico da DLL. Esse endereço pode ser especificado como um número decimal, hexadecimal ou octal.  
  
## <a name="remarks"></a>Comentários  
 O endereço básico padrão de uma DLL é definido pelo Common Language Runtime do .NET Framework.  
  
 Lembre-se de que a palavra de ordem inferior nesse endereço será arredondada. Por exemplo, se 0x11110001 for especificado, será arredondado para 0x11110000.  
  
 Para concluir o processo de assinatura de uma DLL, use SN.EXE com a opção -R.  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Para definir esta opção do compilador no ambiente de desenvolvimento do Visual Studio  
  
1. Abra a página **Propriedades** do projeto.  
  
2. Clique na página de propriedades **Compilar**.  
  
3. Clique no botão **Avançado**.  
  
4. Modifique a propriedade **Endereço Básico de DLL**.  
  
     Para definir programaticamente essa opção do compilador, confira <xref:VSLangProj80.CSharpProjectConfigurationProperties3.BaseAddress%2A>.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Diagnostics.ProcessModule.BaseAddress%2A?displayProperty=nameWithType>
- [Opções do compilador de C#](./index.md)
- [Gerenciando propriedades de solução e de projeto](/visualstudio/ide/managing-project-and-solution-properties)
