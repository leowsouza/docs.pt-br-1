---
title: Configurações do Registro de renderização dos elementos gráficos
ms.date: 03/30/2017
helpviewer_keywords:
- rendering graphics [WPF], registry settings
- rendering graphics [WPF]
- rendering graphics [WPF], troubleshooting
- troubleshooting graphics rendering [WPF]
- graphics [WPF], rendering
ms.assetid: f4b41b42-327d-407c-b398-3ed5f505df8b
ms.openlocfilehash: 0d6eda0aea9ad97063cc5362d83163443de034a6
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976950"
---
# <a name="graphics-rendering-registry-settings"></a>Configurações do Registro de renderização dos elementos gráficos
Este tópico fornece uma visão geral de [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] configurações do Registro de renderização de elementos gráficos que afetam [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] aplicativos.  

<a name="overview"></a>   
## <a name="when-to-use-graphics-rendering-registry-settings"></a>Quando usar Configurações do Registro de renderização dos elementos gráficos  
 Essas configurações do Registro são fornecidas para fins de suporte do produto, depuração e solução de problemas. Como as alterações no Registro afetam todos [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] aplicativos, seu aplicativo nunca deve alterar essas chaves do Registro automaticamente ou durante a instalação.  
  
<a name="xpdmandwddm"></a>   
## <a name="what-are-xpdm-and-wddm"></a>O que são XPDM e WDDM?  
 Algumas das configurações do Registro de renderização de elementos gráficos têm valores padrão diferentes, dependendo se a placa de vídeo usa um driver XPDM ou WDDM. XPDM é o [!INCLUDE[TLA#tla_winxp](../../../../includes/tlasharptla-winxp-md.md)] Display Driver Model e o WDDM é o Windows Display Driver Model. O WDDM está disponível em computadores que executam o Windows Vista e o [!INCLUDE[win7](../../../../includes/win7-md.md)]. O XPDM está disponível em computadores que executam o Windows Vista, [!INCLUDE[TLA#tla_winxp](../../../../includes/tlasharptla-winxp-md.md)]e [!INCLUDE[TLA#tla_winnetsvrfam](../../../../includes/tlasharptla-winnetsvrfam-md.md)]. Para obter mais informações sobre o WDDM, veja [Guia de Design do Windows Vista Display Driver Model](https://go.microsoft.com/fwlink/?LinkId=178394).  
  
<a name="registry_settings"></a>   
## <a name="registry-settings"></a>Configurações do registro  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] fornece quatro configurações do Registro para controlar a renderização [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]:  
  
|Configuração|Descrição|  
|-------------|-----------------|  
|**Desabilitar Opção de Aceleração de hardware**|Especifica se a aceleração de hardware deve ser habilitada.|  
|**Valor máximo de Multisample**|Especifica o grau de multiamostragem para conteúdo de anti-aliasing 3-D.|  
|**Driver de vídeo configuração de data necessário**|Especifica se o sistema desabilita a aceleração de hardware para drivers lançados antes de novembro de 2004.|  
|**Use a opção de rasterizador de referência**|Especifica se [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] deve utilizar o rasterizador de referência.|  
  
 Essas configurações podem ser acessadas por qualquer utilitário de configuração externo que sabe como referenciar as configurações do Registro de [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]. Essas configurações também podem ser criadas ou modificadas acessando os valores diretamente usando o editor do registro do Windows.  
  
<a name="disablehardwareacceleration"></a>   
## <a name="disable-hardware-acceleration-option"></a>Desabilitar Opção de Aceleração de hardware  
  
|Chave do Registro|Tipo de valor|  
|------------------|----------------|  
|`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Avalon.Graphics\DisableHWAcceleration`|DWORD|  
  
 A opção **desabilitar aceleração de hardware** permite que você desative a aceleração de hardware para fins de depuração e teste. Quando você vir a artefatos de renderização em um aplicativo, tente desativar a aceleração de hardware. Se o artefato desaparecer, o problema poderá ser com o driver de vídeo.  
  
 A opção **desabilitar aceleração de hardware** é um valor DWORD 0 ou 1. Um valor de 1 desabilita a aceleração de hardware. Um valor de 0 permite a aceleração de hardware, desde que o sistema atenda aos requisitos de aceleração; para obter mais informações, consulte [Camadas de renderização de elementos gráficos](../advanced/graphics-rendering-tiers.md).  
  
<a name="maxmultisample"></a>   
## <a name="maximum-multisample-value"></a>Valor máximo de Multisample  
  
|Chave do Registro|Tipo de valor|  
|------------------|----------------|  
|`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Avalon.Graphics\MaxMultisampleType`|DWORD|  
  
 O **valor máximo de multiamostrações** permite ajustar a quantidade máxima de anti-aliasing do conteúdo de 3 D. Use esse nível para desabilitar a anti-aliasing 3D no Windows Vista ou habilitá-lo no [!INCLUDE[TLA#tla_winxp](../../../../includes/tlasharptla-winxp-md.md)].  
  
 O **valor de multisample máximo** é um valor DWORD que varia de 0 a 16. Um valor de 0 especifica que a suavização de mutisample de conteúdo 3D deverá ser desabilitada e um valor de 16 tentará usar até 16x a suavização de multisample, se houver suporte da placa de vídeo. Lembre-se de que definir esse valor de chave do registro em computadores que usam drivers XPDM fará com que os aplicativos usem uma grande quantidade de memória de vídeo adicional, diminua o desempenho da renderização 3D e tenha o potencial de introduzir erros de renderização e estabilidade ocorre.  
  
 Quando essa chave do Registro não está definida, [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] volta para o padrão 0 para drivers XPDM e 4 para drivers WDDM.  
  
<a name="requiredvideodriverdatesetting"></a>   
## <a name="required-video-driver-date-setting"></a>Driver de vídeo configuração de data necessário  
  
|Chave do Registro|Tipo de valor|  
|------------------|----------------|  
|`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Avalon.Graphics\RequiredVideoDriverDate`|Cadeia de Caracteres|  
  
 Em novembro de 2004, a Microsoft lançou uma nova versão das diretrizes de teste de driver; os drivers gravados após essa data oferecem melhor estabilidade. Por padrão, [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] usará o pipeline de aceleração de hardware para esses drivers e restabelecerá a renderização de software para drivers XPDM publicados antes desta data.  
  
 A **configuração de data do driver de vídeo necessária** permite que você especifique uma data mínima alternativa para drivers XPDM. Especifique somente uma data anterior a novembro de 2004 se você tiver certeza de que o driver de vídeo é estável o suficiente para oferecer suporte a [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)].  
  
 A configuração do driver de vídeo necessária usa uma cadeia de caracteres de formato a seguir:  
  
| |  
|-|  
|*AAAA* `/` *MM* `/` *DD*|  
  
 Em que *AAAA* é o ano de quatro dígitos, *MM* é o mês de dois dígitos, e *DD* é o dia de dois dígitos. Quando esse valor não for definido, [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] usa de novembro de 2004 como a data do driver de vídeo necessária.  
  
<a name="usereferencerasterizeroption"></a>   
## <a name="use-reference-rasterizer-option"></a>Usar a opção de rasterizador de referência  
  
|Chave do Registro|Tipo de valor|  
|------------------|----------------|  
|`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Avalon.Graphics\UseReferenceRasterizer`|DWORD|  
  
 A **opção usar rasterizador de referência** permite que você force [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] em um modo de renderização de hardware simulado para depuração: [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] entra no modo de hardware, mas usa o rasterizador de software de referência do Microsoft Direct3D, d3dref9. dll, em vez de um dispositivo de hardware real.  
  
 O rasterizador de referência é muito lento, mas ignora o driver de vídeo para evitar problemas de renderização causados por problemas de driver. Por esse motivo, você pode usar o rasterizador de referência para determinar se os problemas de renderização são causados pelo driver de vídeo. O arquivo d3dref9.dll deve estar em um local onde o aplicativo pode acessá-lo, como em qualquer local no caminho do sistema ou no diretório local do aplicativo.  
  
 A opção **usar rasterizador de referência** assume um valor DWORD. Um valor de 0 indica que o rasterizador de referência não é usado. Qualquer outro valor diferente de zero força [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] para usar o rasterizador de referência.  
  
## <a name="see-also"></a>Consulte também

- [Camadas de renderização de gráficos](../advanced/graphics-rendering-tiers.md)
- [Visão geral de renderização de gráficos do WPF](wpf-graphics-rendering-overview.md)
