---
title: Delegação e representação com o WCF
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- impersonation [WCF]
- delegation [WCF]
ms.assetid: 110e60f7-5b03-4b69-b667-31721b8e3152
ms.openlocfilehash: 39b71d3b5cbcfdc8bde3449560587f033c437d50
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70856158"
---
# <a name="delegation-and-impersonation-with-wcf"></a>Delegação e representação com o WCF
A *representação* é uma técnica comum que os serviços usam para restringir o acesso do cliente aos recursos de um domínio de serviço. Os recursos de domínio de serviço podem ser recursos de máquina, como arquivos locais (representação) ou um recurso em outra máquina, como um compartilhamento de arquivos (delegação). Para um aplicativo de exemplo, consulte [personificando o cliente](../../../../docs/framework/wcf/samples/impersonating-the-client.md). Para obter um exemplo de como usar a representação, [consulte Como: Representar um cliente em um serviço](../../../../docs/framework/wcf/how-to-impersonate-a-client-on-a-service.md).  
  
> [!IMPORTANT]
> Lembre-se de que, ao representar um cliente em um serviço, o serviço é executado com as credenciais do cliente, o que pode ter privilégios mais altos do que o processo do servidor.  
  
## <a name="overview"></a>Visão geral  
 Normalmente, os clientes chamam um serviço para que o serviço execute alguma ação em nome do cliente. A representação permite que o serviço atue como o cliente ao executar a ação. A delegação permite que um serviço de front-end encaminhe a solicitação do cliente para um serviço de back-end de forma que o serviço de back-end também possa representar o cliente. A representação é mais comumente usada como uma maneira de verificar se um cliente está autorizado a executar uma ação específica, enquanto a delegação é uma maneira de fluir recursos de representação, juntamente com a identidade do cliente, para um serviço de back-end. A delegação é um recurso de domínio do Windows que pode ser usado quando a autenticação baseada em Kerberos é executada. A delegação é distinta do fluxo de identidade e, como a delegação transfere a capacidade de representar o cliente sem a posse da senha do cliente, é uma operação com privilégios muito maior do que o fluxo de identidade.  
  
 A representação e a delegação exigem que o cliente tenha uma identidade do Windows. Se um cliente não possuir uma identidade do Windows, a única opção disponível é fluir a identidade do cliente para o segundo serviço.  
  
## <a name="impersonation-basics"></a>Noções básicas sobre representação  
 O Windows Communication Foundation (WCF) dá suporte à representação de uma variedade de credenciais de cliente. Este tópico descreve o suporte ao modelo de serviço para representar o chamador durante a implementação de um método de serviço. Também são discutidos cenários comuns de implantação envolvendo a representação e a segurança SOAP e as opções do WCF nesses cenários.  
  
 Este tópico enfoca a representação e a delegação no WCF ao usar a segurança SOAP. Você também pode usar a representação e a delegação com o WCF ao usar a segurança de transporte, conforme descrito em [usando a representação com segurança de transporte](../../../../docs/framework/wcf/feature-details/using-impersonation-with-transport-security.md).  
  
## <a name="two-methods"></a>Dois métodos  
 A segurança SOAP do WCF tem dois métodos distintos para executar a representação. O método usado depende da associação. Uma é a representação de um token do Windows obtido da autenticação de SSPI (Security Support Provider interface) ou Kerberos, que é então armazenada em cache no serviço. A segunda é a representação de um token do Windows obtido das extensões Kerberos, coletivamente chamado de *serviço para usuário* (S4U).  
  
### <a name="cached-token-impersonation"></a>Representação de token em cache  
 Você pode executar a representação em cache-token com o seguinte:  
  
- <xref:System.ServiceModel.WSHttpBinding>, <xref:System.ServiceModel.WSDualHttpBinding> e<xref:System.ServiceModel.NetTcpBinding> com uma credencial de cliente do Windows.  
  
- <xref:System.ServiceModel.BasicHttpBinding>com um <xref:System.ServiceModel.BasicHttpSecurityMode> conjunto para a <xref:System.ServiceModel.BasicHttpSecurityMode.TransportWithMessageCredential> credencial ou qualquer outra associação padrão em que o cliente apresenta uma credencial de nome de usuário que o serviço pode mapear para uma conta válida do Windows.  
  
- Qualquer <xref:System.ServiceModel.Channels.CustomBinding> que usa uma credencial de cliente do `requireCancellation` Windows com `true`o definido como. (A propriedade está disponível nas seguintes classes: <xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters>, <xref:System.ServiceModel.Security.Tokens.SslSecurityTokenParameters>e <xref:System.ServiceModel.Security.Tokens.SspiSecurityTokenParameters>.) Se uma conversa segura for usada na associação, ela também deverá ter a `requireCancellation` propriedade definida como. `true`  
  
- Qualquer <xref:System.ServiceModel.Channels.CustomBinding> onde o cliente apresenta uma credencial de nome de usuário. Se a conversa segura for usada na associação, ela também deverá ter a `requireCancellation` propriedade definida como `true`.  
  
### <a name="s4u-based-impersonation"></a>Representação baseada em S4U  
 Você pode executar a representação baseada em S4U com o seguinte:  
  
- <xref:System.ServiceModel.WSHttpBinding>, <xref:System.ServiceModel.WSDualHttpBinding> e<xref:System.ServiceModel.NetTcpBinding> com uma credencial de cliente de certificado que o serviço pode mapear para uma conta válida do Windows.  
  
- Qualquer <xref:System.ServiceModel.Channels.CustomBinding> que usa uma credencial de cliente do `requireCancellation` Windows com a `false`propriedade definida como.  
  
- Qualquer <xref:System.ServiceModel.Channels.CustomBinding> um que use um nome de usuário ou uma credencial de cliente do `requireCancellation` Windows e uma `false`conversa segura com a propriedade definida como.  
  
 A extensão até a qual o serviço pode representar o cliente depende dos privilégios que a conta de serviço mantém quando tenta a representação, o tipo de representação usada e, possivelmente, a extensão da representação que o cliente permite.  
  
> [!NOTE]
> Quando o cliente e o serviço estão em execução no mesmo computador e o cliente está sendo executado em uma conta do sistema ( `Local System` por `Network Service`exemplo, ou), o cliente não pode ser representado quando uma sessão segura é estabelecida com o contexto de segurança com estado token. Um aplicativo de formulário ou de console do Windows normalmente é executado na conta conectada no momento, para que essa conta possa ser representada por padrão. No entanto, quando o cliente é uma página ASP.net e essa página é hospedada no IIS 6,0 ou no IIS 7,0, o cliente executa `Network Service` a execução na conta por padrão. Todas as associações fornecidas pelo sistema que dão suporte a sessões seguras usam um símbolo de contexto de segurança sem estado (SCT) por padrão. No entanto, se o cliente for uma página ASP.NET e as sessões seguras com estado SCTs forem usadas, o cliente não poderá ser representado. Para obter mais informações sobre como usar SCTs com estado em uma sessão [segura, consulte Como: Crie um token de contexto de segurança para uma](../../../../docs/framework/wcf/feature-details/how-to-create-a-security-context-token-for-a-secure-session.md)sessão segura.  
  
## <a name="impersonation-in-a-service-method-declarative-model"></a>Representação em um método de serviço: Modelo declarativo  
 A maioria dos cenários de representação envolve a execução do método de serviço no contexto do chamador. O WCF fornece um recurso de representação que facilita essa tarefa, permitindo que o usuário especifique o requisito de representação no <xref:System.ServiceModel.OperationBehaviorAttribute> atributo. Por exemplo, no código a seguir, a infraestrutura do WCF representa o chamador antes de executar `Hello` o método. Qualquer tentativa de acessar recursos nativos dentro do `Hello` método terá sucesso apenas se a ACL (lista de controle de acesso) do recurso permitir os privilégios de acesso do chamador. Para habilitar a representação, defina <xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A> a propriedade como um <xref:System.ServiceModel.ImpersonationOption> dos valores de enumeração, <xref:System.ServiceModel.ImpersonationOption.Required?displayProperty=nameWithType> ou <xref:System.ServiceModel.ImpersonationOption.Allowed?displayProperty=nameWithType>, conforme mostrado no exemplo a seguir.  
  
> [!NOTE]
> Quando um serviço tiver credenciais mais altas do que o cliente remoto, as credenciais do serviço serão usadas se <xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A> a propriedade for definida <xref:System.ServiceModel.ImpersonationOption.Allowed>como. Ou seja, se um usuário com poucos privilégios fornecer suas credenciais, um serviço com privilégios elevados executará o método com as credenciais do serviço e poderá usar recursos que o usuário com poucos privilégios não poderá usar.  
  
 [!code-csharp[c_ImpersonationAndDelegation#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_impersonationanddelegation/cs/source.cs#1)]
 [!code-vb[c_ImpersonationAndDelegation#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_impersonationanddelegation/vb/source.vb#1)]  
  
 A infraestrutura do WCF pode representar o chamador somente se o chamador for autenticado com credenciais que podem ser mapeadas para uma conta de usuário do Windows. Se o serviço estiver configurado para autenticar usando uma credencial que não pode ser mapeada para uma conta do Windows, o método de serviço não será executado.  
  
> [!NOTE]
> Em [!INCLUDE[wxp](../../../../includes/wxp-md.md)], a representação falhará se um SCT com estado for criado, <xref:System.InvalidOperationException>resultando em um. Para obter mais informações, consulte [cenários sem suporte](../../../../docs/framework/wcf/feature-details/unsupported-scenarios.md).  
  
## <a name="impersonation-in-a-service-method-imperative-model"></a>Representação em um método de serviço: Modelo imperativo  
 Às vezes, um chamador não precisa representar o método de serviço inteiro para funcionar, mas apenas para uma parte dele. Nesse caso, obtenha a identidade do Windows do chamador dentro do método de serviço e execute imperativamente a representação. Faça isso usando <xref:System.ServiceModel.ServiceSecurityContext.WindowsIdentity%2A> a propriedade <xref:System.ServiceModel.ServiceSecurityContext> de para <xref:System.Security.Principal.WindowsIdentity> retornar uma instância da classe e chamar o <xref:System.Security.Principal.WindowsIdentity.Impersonate%2A> método antes de usar a instância.  
  
> [!NOTE]
> Certifique-se de usar a`Using` instrução Visual Basic ou C# `using` a instrução para reverter automaticamente a ação de representação. Se você não usar a instrução ou se usar uma linguagem de programação diferente de Visual Basic ou C#, certifique-se de reverter o nível de representação. A falha de fazer isso pode formar a base para a negação de serviço e a elevação de ataques de privilégio.  
  
 [!code-csharp[c_ImpersonationAndDelegation#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_impersonationanddelegation/cs/source.cs#2)]
 [!code-vb[c_ImpersonationAndDelegation#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_impersonationanddelegation/vb/source.vb#2)]  
  
## <a name="impersonation-for-all-service-methods"></a>Representação para todos os métodos de serviço  
 Em alguns casos, você deve executar todos os métodos de um serviço no contexto do chamador. Em vez de habilitar explicitamente esse recurso em uma base por método, use o <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>. Conforme mostrado no código a seguir, defina a <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.ImpersonateCallerForAllOperations%2A> Propriedade como `true`. O <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> é recuperado das coleções de comportamentos <xref:System.ServiceModel.ServiceHost> da classe. Observe também que a `Impersonation` propriedade <xref:System.ServiceModel.OperationBehaviorAttribute> do aplicado a cada método <xref:System.ServiceModel.ImpersonationOption.Allowed> também deve ser definida como ou <xref:System.ServiceModel.ImpersonationOption.Required>.  
  
 [!code-csharp[c_ImpersonationAndDelegation#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_impersonationanddelegation/cs/source.cs#3)]
 [!code-vb[c_ImpersonationAndDelegation#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_impersonationanddelegation/vb/source.vb#3)]  
  
 A tabela a seguir descreve o comportamento do WCF para todas `ImpersonationOption` as `ImpersonateCallerForAllServiceOperations`combinações possíveis de e.  
  
|`ImpersonationOption`|`ImpersonateCallerForAllServiceOperations`|Comportamento|  
|---------------------------|------------------------------------------------|--------------|  
|Necessária|N/D|O WCF representa o chamador|  
|Permitidos|false|O WCF não representa o chamador|  
|Permitidos|true|O WCF representa o chamador|  
|NotAllowed|false|O WCF não representa o chamador|  
|NotAllowed|true|Permitida. (Um <xref:System.InvalidOperationException> é lançado.)|  
  
## <a name="impersonation-level-obtained-from-windows-credentials-and-cached-token-impersonation"></a>Nível de representação obtido de credenciais do Windows e representação de token em cache  
 Em alguns cenários, o cliente tem controle parcial sobre o nível de representação que o serviço executa quando uma credencial de cliente do Windows é usada. Um cenário ocorre quando o cliente especifica um nível de representação anônima. O outro ocorre ao executar a representação com um token armazenado em cache. Isso é feito definindo a <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> propriedade <xref:System.ServiceModel.Security.WindowsClientCredential> da classe, que é acessada como uma propriedade da classe genérica <xref:System.ServiceModel.ChannelFactory%601> .  
  
> [!NOTE]
> Especificar um nível de representação de anônimo faz com que o cliente faça logon no serviço anonimamente. Portanto, o serviço deve permitir logons anônimos, independentemente de a representação ser executada.  
  
 O cliente pode especificar o nível de representação <xref:System.Security.Principal.TokenImpersonationLevel.Anonymous>como <xref:System.Security.Principal.TokenImpersonationLevel.Identification> <xref:System.Security.Principal.TokenImpersonationLevel.Impersonation>,, ou <xref:System.Security.Principal.TokenImpersonationLevel.Delegation>. Somente um token no nível especificado é produzido, conforme mostrado no código a seguir.  
  
 [!code-csharp[c_ImpersonationAndDelegation#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_impersonationanddelegation/cs/source.cs#4)]
 [!code-vb[c_ImpersonationAndDelegation#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_impersonationanddelegation/vb/source.vb#4)]  
  
 A tabela a seguir especifica o nível de representação que o serviço obtém ao representar um token armazenado em cache.  
  
|Valor `AllowedImpersonationLevel`|O serviço tem`SeImpersonatePrivilege`|O serviço e o cliente são capazes de delegação|Token em cache`ImpersonationLevel`|  
|---------------------------------------|------------------------------------------|--------------------------------------------------|---------------------------------------|  
|Modo|Sim|N/D|Representação|  
|Modo|Não|N/D|ID|  
|ID|N/D|N/D|ID|  
|Representação|Sim|N/D|Representação|  
|Representação|Não|N/D|ID|  
|Delegação|Sim|Sim|Delegação|  
|Delegação|Sim|Não|Representação|  
|Delegação|Não|N/D|ID|  
  
## <a name="impersonation-level-obtained-from-user-name-credentials-and-cached-token-impersonation"></a>Nível de representação obtido de credenciais de nome de usuário e representação de token em cache  
 Ao passar o serviço seu nome de usuário e senha, um cliente permite que o WCF faça logon como esse usuário, o que equivale a `AllowedImpersonationLevel` definir a <xref:System.Security.Principal.TokenImpersonationLevel.Delegation>Propriedade como. (O `AllowedImpersonationLevel` está disponível <xref:System.ServiceModel.Security.WindowsClientCredential> nas classes e <xref:System.ServiceModel.Security.HttpDigestClientCredential> .) A tabela a seguir fornece o nível de representação obtido quando o serviço recebe credenciais de nome de usuário.  
  
|`AllowedImpersonationLevel`|O serviço tem`SeImpersonatePrivilege`|O serviço e o cliente são capazes de delegação|Token em cache`ImpersonationLevel`|  
|---------------------------------|------------------------------------------|--------------------------------------------------|---------------------------------------|  
|N/D|Sim|Sim|Delegação|  
|N/D|Sim|Não|Representação|  
|N/D|Não|N/D|ID|  
  
## <a name="impersonation-level-obtained-from-s4u-based-impersonation"></a>Nível de representação obtido da representação baseada em S4U  
  
|O serviço tem`SeTcbPrivilege`|O serviço tem`SeImpersonatePrivilege`|O serviço e o cliente são capazes de delegação|Token em cache`ImpersonationLevel`|  
|----------------------------------|------------------------------------------|--------------------------------------------------|---------------------------------------|  
|Sim|Sim|N/D|Representação|  
|Sim|Não|N/D|ID|  
|Não|N/D|N/D|ID|  
  
## <a name="mapping-a-client-certificate-to-a-windows-account"></a>Mapeando um certificado de cliente para uma conta do Windows  
 É possível que um cliente se autentique em um serviço usando um certificado e faça com que o serviço mapeie o cliente para uma conta existente por meio de Active Directory. O XML a seguir mostra como configurar o serviço para mapear o certificado.  
  
```xml  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="MapToWindowsAccount">  
      <serviceCredentials>  
        <clientCertificate>  
          <authentication mapClientCertificateToWindowsAccount="true" />  
        </clientCertificate>  
      </serviceCredentials>  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
 O código a seguir mostra como configurar o serviço.  
  
```csharp
// Create a binding that sets a certificate as the client credential type.  
WSHttpBinding b = new WSHttpBinding();  
b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;  
  
// Create a service host that maps the certificate to a Windows account.  
Uri httpUri = new Uri("http://localhost/Calculator");  
ServiceHost sh = new ServiceHost(typeof(HelloService), httpUri);  
sh.Credentials.ClientCertificate.Authentication.MapClientCertificateToWindowsAccount = true;  
```  
  
## <a name="delegation"></a>Delegação  
 Para delegar a um serviço de back-end, um serviço deve executar o Kerberos multi-perna (SSPI sem o fallback de NTLM) ou a autenticação Kerberos direta para o serviço de back-end usando a identidade do Windows do cliente. Para delegar a um serviço de back-end, <xref:System.ServiceModel.ChannelFactory%601> crie um e um canal e, em seguida, comunique-se por meio do canal ao representar o cliente. Com essa forma de delegação, a distância na qual o serviço de back-end pode ser localizado do serviço de front-end depende do nível de representação obtido pelo serviço de front-end. Quando o nível de representação <xref:System.Security.Principal.TokenImpersonationLevel.Impersonation>é, os serviços de front-end e back-end devem estar em execução no mesmo computador. Quando o nível de representação <xref:System.Security.Principal.TokenImpersonationLevel.Delegation>é, os serviços de front-end e back-end podem estar em computadores separados ou no mesmo computador. Habilitar a representação em nível de delegação requer que a política de domínio do Windows seja configurada para permitir a delegação. Para obter mais informações sobre como configurar Active Directory para suporte à delegação, consulte [habilitando a autenticação delegada](https://go.microsoft.com/fwlink/?LinkId=99690).  
  
> [!NOTE]
> Quando um cliente é autenticado no serviço de front-end usando um nome de usuário e senha que correspondem a uma conta do Windows no serviço de back-end, o serviço de front-end pode se autenticar no serviço de back-end reutilizando o nome de usuário e a senha do cliente. Essa é uma forma particularmente poderosa de fluxo de identidade, pois passar o nome de usuário e a senha para o serviço de back-end permite que o serviço de back-end execute a representação, mas não constitui a delegação porque o Kerberos não é usado. Os controles de Active Directory na delegação não se aplicam ao nome de usuário e à autenticação de senha.  
  
### <a name="delegation-ability-as-a-function-of-impersonation-level"></a>Capacidade de delegação como uma função de nível de representação  
  
|Nível de representação|O serviço pode executar a delegação entre processos|O serviço pode executar a delegação entre computadores|  
|-------------------------|---------------------------------------------------|---------------------------------------------------|  
|<xref:System.Security.Principal.TokenImpersonationLevel.Identification>|Não|Não|  
|<xref:System.Security.Principal.TokenImpersonationLevel.Impersonation>|Sim|Não|  
|<xref:System.Security.Principal.TokenImpersonationLevel.Delegation>|Sim|Sim|  
  
 O exemplo de código a seguir demonstra como usar a delegação.  
  
 [!code-csharp[c_delegation#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_delegation/cs/source.cs#1)]
 [!code-vb[c_delegation#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_delegation/vb/source.vb#1)]  
  
### <a name="how-to-configure-an-application-to-use-constrained-delegation"></a>Como configurar um aplicativo para usar a delegação restrita  
 Antes de poder usar a delegação restrita, o remetente, o destinatário e o controlador de domínio devem ser configurados para fazer isso. O procedimento a seguir lista as etapas que habilitam a delegação restrita. Para obter detalhes sobre as diferenças entre delegação e delegação restrita, consulte a parte das [extensões Kerberos do Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=100194) que aborda a discussão restrita.  
  
1. No controlador de domínio, desmarque a caixa de seleção a **conta é confidencial e não pode ser delegada** para a conta sob a qual o aplicativo cliente está sendo executado.  
  
2. No controlador de domínio, marque a caixa de seleção a **conta é confiável para delegação** para a conta sob a qual o aplicativo cliente está sendo executado.  
  
3. No controlador de domínio, configure o computador de camada intermediária para que ele seja confiável para delegação, clicando na opção **confiar no computador para delegação** .  
  
4. No controlador de domínio, configure o computador de camada intermediária para usar a delegação restrita, clicando na opção **confiar neste computador para delegação a serviços especificados** .  
  
 Para obter instruções mais detalhadas sobre como configurar a delegação restrita, consulte os seguintes tópicos no MSDN:  
  
- [Solucionando problemas de delegação de Kerberos](https://go.microsoft.com/fwlink/?LinkId=36724)  
  
- [Transição de protocolo Kerberos e delegação restrita](https://go.microsoft.com/fwlink/?LinkId=36725)  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.OperationBehaviorAttribute>
- <xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A>
- <xref:System.ServiceModel.ImpersonationOption>
- <xref:System.ServiceModel.ServiceSecurityContext.WindowsIdentity%2A>
- <xref:System.ServiceModel.ServiceSecurityContext>
- <xref:System.Security.Principal.WindowsIdentity>
- <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>
- <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.ImpersonateCallerForAllOperations%2A>
- <xref:System.ServiceModel.ServiceHost>
- <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A>
- <xref:System.ServiceModel.Security.WindowsClientCredential>
- <xref:System.ServiceModel.ChannelFactory%601>
- <xref:System.Security.Principal.TokenImpersonationLevel.Identification>
- [Usando a representação com segurança de transporte](../../../../docs/framework/wcf/feature-details/using-impersonation-with-transport-security.md)
- [Representar o cliente](../../../../docs/framework/wcf/samples/impersonating-the-client.md)
- [Como: Representar um cliente em um serviço](../../../../docs/framework/wcf/how-to-impersonate-a-client-on-a-service.md)
- [Ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)
