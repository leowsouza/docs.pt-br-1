---
title: 'Ponto de extremidade: Falhas de Validação e Autenticação de Segurança por Segundo'
ms.date: 03/30/2017
ms.assetid: 89a70b90-d7e4-4b03-9b84-4dc88ce3d605
ms.openlocfilehash: a6d76a0d11c52d20aebd44a85862c802cc0a68f7
ms.sourcegitcommit: e08b319358a8025cc6aa38737854f7bdb87183d6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64912476"
---
# <a name="endpoint-security-validation-and-authentication-failures-per-second"></a>Ponto de extremidade: Falhas de Validação e Autenticação de Segurança por Segundo
Nome do contador: Falhas de Validação e Autenticação de Segurança por Segundo  
  
## <a name="description"></a>Descrição  
 Esse contador é incrementado sempre que uma mensagem foi rejeitada devido a um problema de segurança não coberto pelo contador "Chamadas de segurança não autorizado". Esses problemas incluem:  
  
- Não é possível ler o token de cliente da mensagem.  
  
- Token de cliente falha na autenticação (por exemplo, senha incorreta).  
  
- Falha na verificação de assinatura (por exemplo, a mensagem foi violada).  
  
- A mensagem é uma duplicata da anterior, o que pode ocorrer durante um ataque de repetição.  
  
- Ocorreu uma falha de descriptografia.  
  
- Alguns necessários elementos (por exemplo, carimbo de hora ausente ou bloquear os dados criptografados) estão ausentes da mensagem.  
  
- Ocorreram erros durante o handshake TLSNEGO/SPNEGO.  
  
 Esse contador é do tipo de contador de desempenho [PERF_COUNTER_COUNTER](https://go.microsoft.com/fwlink/?LinkID=94649), cujo valor é calculado usando a fórmula a seguir:  
  
 (N1-N0)/((D1-D0)/F)
