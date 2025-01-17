---
title: Método ICorProfilerInfo2::DoStackSnapshot
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.DoStackSnapshot
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::DoStackSnapshot
helpviewer_keywords:
- ICorProfilerInfo2::DoStackSnapshot method [.NET Framework profiling]
- DoStackSnapshot method [.NET Framework profiling]
ms.assetid: 287b11e9-7c52-4a13-ba97-751203fa97f4
topic_type:
- apiref
ms.openlocfilehash: 64bcf6ee58d743a26e31c49a425f36cc808b5080
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74426831"
---
# <a name="icorprofilerinfo2dostacksnapshot-method"></a>Método ICorProfilerInfo2::DoStackSnapshot
Percorre os quadros gerenciados na pilha para o thread especificado e envia informações para o criador de perfil por meio de um retorno de chamada.  
  
## <a name="syntax"></a>Sintaxe  
  
```cpp  
HRESULT DoStackSnapshot(  
    [in] ThreadID thread,  
    [in] StackSnapshotCallback *callback,  
    [in] ULONG32 infoFlags,  
    [in] void *clientData,  
    [in, size_is(contextSize), length_is(contextSize)] BYTE context[],  
    [in] ULONG32 contextSize);  
```  
  
## <a name="parameters"></a>Parâmetros  
 `thread`  
 no A ID do thread de destino.  
  
 Passar NULL em `thread` gera um instantâneo do thread atual. Se um `ThreadID` de um thread diferente for passado, o Common Language Runtime (CLR) suspenderá esse thread, executará o instantâneo e será retomado.  
  
 `callback`  
 no Um ponteiro para a implementação do método [StackSnapshotCallback](../../../../docs/framework/unmanaged-api/profiling/stacksnapshotcallback-function.md) , que é chamado pelo CLR para fornecer ao criador de perfil informações sobre cada quadro gerenciado e cada execução de quadros não gerenciados.  
  
 O método `StackSnapshotCallback` é implementado pelo gravador do criador de perfil.  
  
 `infoFlags`  
 no Um valor da enumeração [COR_PRF_SNAPSHOT_INFO](../../../../docs/framework/unmanaged-api/profiling/cor-prf-snapshot-info-enumeration.md) , que especifica a quantidade de dados a serem passados de volta para cada quadro por `StackSnapshotCallback`.  
  
 `clientData`  
 no Um ponteiro para os dados do cliente, que é passado diretamente para a função de retorno de chamada `StackSnapshotCallback`.  
  
 `context`  
 no Um ponteiro para uma estrutura de `CONTEXT` do Win32, que é usado para propagar a movimentação da pilha. A estrutura de `CONTEXT` do Win32 contém valores dos registros de CPU e representa o estado da CPU em um determinado momento no tempo.  
  
 A semente ajuda o CLR a determinar onde começar a movimentação da pilha, se a parte superior da pilha for um código auxiliar não gerenciado; caso contrário, a semente será ignorada. Uma semente deve ser fornecida para uma movimentação assíncrona. Se você estiver fazendo uma movimentação síncrona, nenhuma semente será necessária.  
  
 O parâmetro `context` é válido somente se o sinalizador COR_PRF_SNAPSHOT_CONTEXT foi passado no parâmetro `infoFlags`.  
  
 `contextSize`  
 no O tamanho da estrutura de `CONTEXT`, que é referenciada pelo parâmetro `context`.  
  
## <a name="remarks"></a>Comentários  
 Passar NULL por `thread` produz um instantâneo do thread atual. Os instantâneos podem ser tirados de outros threads somente se o thread de destino for suspenso no momento.  
  
 Quando o criador de perfil deseja percorrer a pilha, ele chama `DoStackSnapshot`. Antes que o CLR retorne dessa chamada, ele chama o `StackSnapshotCallback` várias vezes, uma vez para cada quadro gerenciado (ou execução de quadros não gerenciados) na pilha. Quando são encontrados quadros não gerenciados, você mesmo deve acompanhá-los.  
  
 A ordem na qual a pilha é movimentada é o inverso de como os quadros foram enviados para a pilha: quadro (último a ser enviado) primeiro, principal (primeiro a ser enviado).  
  
 Para obter mais informações sobre como programar o criador de perfil para movimentar pilhas gerenciadas, consulte [movimentação de pilha do profiler no .NET Framework 2,0: Noções básicas e além disso](https://go.microsoft.com/fwlink/?LinkId=73638).  
  
 Uma movimentação de pilha pode ser síncrona ou assíncrona, conforme explicado nas seções a seguir.  
  
## <a name="synchronous-stack-walk"></a>Movimentação de pilha síncrona  
 Uma movimentação de pilha síncrona envolve percorrer a pilha do thread atual em resposta a um retorno de chamada. Ele não requer propagação ou suspensão.  
  
 Você faz uma chamada síncrona quando, em resposta ao CLR chamando um dos métodos [ICorProfilerCallback](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-interface.md) (ou [ICorProfilerCallback2](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback2-interface.md)) de seu criador de perfil, você chama `DoStackSnapshot` para percorrer a pilha do thread atual. Isso é útil quando você deseja ver qual é a aparência da pilha em uma notificação como [ICorProfilerCallback:: ObjectAllocated](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-objectallocated-method.md). Basta chamar `DoStackSnapshot` de dentro de seu método `ICorProfilerCallback`, passando NULL nos parâmetros `context` e `thread`.  
  
## <a name="asynchronous-stack-walk"></a>Movimentação de pilha assíncrona  
 Uma movimentação de pilha assíncrona envolve a movimentação da pilha de um thread diferente ou a movimentação da pilha do thread atual, não em resposta a um retorno de chamada, mas ao seqüestrar o ponteiro de instrução do thread atual. Uma movimentação assíncrona exigirá uma semente se a parte superior da pilha for um código não gerenciado que não faz parte de uma chamada de plataforma (PInvoke) ou de chamadas COM, mas o código auxiliar no próprio CLR. Por exemplo, o código que faz compilação JIT (just-in-time) ou coleta de lixo é o código auxiliar.  
  
 Você Obtém uma semente suspendendo diretamente o thread-alvo e movimentando sua pilha por conta própria até encontrar o quadro gerenciado superior. Depois que o thread de destino for suspenso, obtenha o contexto de registro atual do thread de destino. Em seguida, determine se o contexto de registro aponta para código não gerenciado chamando [ICorProfilerInfo:: GetFunctionFromIP](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo-getfunctionfromip-method.md) — se ele retornar um `FunctionID` igual a zero, o quadro será um código não gerenciado. Agora, percorra a pilha até atingir o primeiro quadro gerenciado e, em seguida, calcule o contexto de semente com base no contexto de registro desse quadro.  
  
 Chame `DoStackSnapshot` com o contexto de semente para iniciar a movimentação de pilha assíncrona. Se você não fornecer uma semente, `DoStackSnapshot` poderá ignorar os quadros gerenciados na parte superior da pilha e, consequentemente, fornecerá uma movimentação de pilha incompleta. Se você fornecer uma semente, ele deverá apontar para código gerado por JIT ou gerador de imagem nativa (NGen. exe); caso contrário, `DoStackSnapshot` retornará o código de falha, CORPROF_E_STACKSNAPSHOT_UNMANAGED_CTX.  
  
 As movimentações assíncronas podem causar facilmente deadlocks ou violações de acesso, a menos que você siga estas diretrizes:  
  
- Quando você suspende threads diretamente, lembre-se de que apenas um thread que nunca executou código gerenciado pode suspender outro thread.  
  
- Bloquear sempre em seu retorno de chamada [ICorProfilerCallback:: ThreadDestroyed](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-threaddestroyed-method.md) até que a movimentação da pilha desse thread seja concluída.  
  
- Não mantenha um bloqueio enquanto o criador de perfil chamar uma função CLR que possa disparar uma coleta de lixo. Ou seja, não mantenha um bloqueio se o thread de propriedade puder fazer uma chamada que dispara uma coleta de lixo.  
  
 Também há um risco de deadlock se você chamar `DoStackSnapshot` de um thread que seu criador de perfil criou para que você possa percorrer a pilha de um thread-alvo separado. Na primeira vez que o thread que você criou insere determinados métodos de `ICorProfilerInfo*` (incluindo `DoStackSnapshot`), o CLR executará a inicialização específica do CLR por thread nesse thread. Se o seu criador de perfil tiver suspenso o thread de destino cuja pilha você está tentando percorrer e se esse thread de destino tiver causado um bloqueio necessário para executar essa inicialização por thread, ocorrerá um deadlock. Para evitar esse deadlock, faça uma chamada inicial em `DoStackSnapshot` de seu thread criado pelo criador de perfil para percorrer um thread de destino separado, mas não suspenda o thread-alvo primeiro. Essa chamada inicial garante que a inicialização por thread possa ser concluída sem deadlock. Se `DoStackSnapshot` tiver sucesso e reportar pelo menos um quadro, depois desse ponto, será seguro que esse thread criado pelo criador de perfil suspenda qualquer thread de destino e chame `DoStackSnapshot` para percorrer a pilha do thread-alvo.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** confira [Requisitos do sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Cabeçalho:** CorProf. idl, CorProf. h  
  
 **Biblioteca:** CorGuids.lib  
  
 **Versões do .NET Framework:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Interface ICorProfilerInfo](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo-interface.md)
- [Interface ICorProfilerInfo2](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo2-interface.md)
