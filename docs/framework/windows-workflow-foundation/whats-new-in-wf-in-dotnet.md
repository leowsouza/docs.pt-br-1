---
title: Novidades no Windows Foundation Workflow no .NET 4.5
ms.date: 03/30/2017
ms.assetid: 195c43a8-e0a8-43d9-aead-d65a9e6751ec
ms.openlocfilehash: 0244457a051740f37c11c48f41d98bdb2d741aec
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2019
ms.locfileid: "74142027"
---
# <a name="whats-new-in-windows-workflow-foundation-in-net-45"></a>Novidades no Windows Foundation Workflow no .NET 4.5

Windows Workflow Foundation (WF) in .NET Framework 4.5 introduces many new features, such as new activities, designer capabilities, and workflow development models. Many, but not all, of the new workflow features introduced in .NET Framework 4.5 are supported in the re-hosted workflow designer. For more information about the new features that are supported, see [Support for New Workflow Foundation 4.5 Features in the Rehosted Workflow Designer](wf-features-in-the-rehosted-workflow-designer.md). For more information about migrating .NET 3.0 and .NET 3.5 workflow applications to use the latest version, see [Migration Guidance](migration-guidance.md). This topic provides an overview of the new workflow features introduced in .NET Framework 4.5.

> [!WARNING]
> The new Windows Workflow Foundation features introduced in .NET Framework 4.5 are not available for projects that target previous versions of the framework. If a project that targets .NET Framework 4.5 is re-targeted to a previous version of the framework, several issues can occur.
>
> - C# expressions will be replaced in the designer with the message **Value was set in XAML**.
> - Muitos erros de compilação ocorrerão, incluindo o seguinte.
>
> **The file format is not compatible with current targeting framework. To convert the file format, please explicitly save the file. This error message will go away after you save the file and reopen the designer.**

## <a name="BKMK_Versioning"></a> Workflow Versioning

.NET Framework 4.5 introduced several new versioning features based around the new <xref:System.Activities.WorkflowIdentity> class. O <xref:System.Activities.WorkflowIdentity> fornece a autores de aplicativo de fluxo de trabalho um mecanismo para mapear uma instância de fluxo de trabalho persistida com sua definição.

- Os desenvolvedores que usam a hospedagem do <xref:System.Activities.WorkflowApplication> podem usar o <xref:System.Activities.WorkflowIdentity> para habilitar hospedagem de várias versões de um fluxo de trabalho lado a lado. As instâncias de fluxo de trabalho persistidas podem ser carregadas usando a nova classe <xref:System.Activities.WorkflowApplicationInstance> e, em seguida, o <xref:System.Activities.WorkflowApplicationInstance.DefinitionIdentity%2A> pode ser usado pelo host para fornecer a versão correta da definição de fluxo de trabalho ao criar uma instância do <xref:System.Activities.WorkflowApplication>. For more information, see [Using WorkflowIdentity and Versioning](using-workflowidentity-and-versioning.md) and [How to: Host Multiple Versions of a Workflow Side-by-Side](how-to-host-multiple-versions-of-a-workflow-side-by-side.md).

- O <xref:System.ServiceModel.WorkflowServiceHost> agora é um host de várias versões. Quando uma nova versão de um serviço de fluxo de trabalho é implantada, as novas instâncias são criadas usando o novo serviço, mas as instâncias existentes continuam usando a versão anterior. For more information, see [Side by Side Versioning in WorkflowServiceHost](../wcf/feature-details/side-by-side-versioning-in-workflowservicehost.md).

- A atualização dinâmica é introduzida, o que fornece um mecanismo para atualizar a definição de uma instância de fluxo de trabalho persistida. For more information, see [Dynamic Update](dynamic-update.md) and [How to: Update the Definition of a Running Workflow Instance](how-to-update-the-definition-of-a-running-workflow-instance.md).

- A SqlWorkflowInstanceStoreSchemaUpgrade.sql database script is provided to upgrade persistence databases created using the .NET Framework 4 database scripts. This script updates .NET Framework 4 persistence databases to support the new versioning capabilities introduced in .NET Framework 4.5. As instâncias de fluxo de trabalho persistidas no banco de dados recebem valores padrão do controle de versão e podem participar da execução lado a lado e da atualização dinâmica. For more information, see [Upgrading .NET Framework 4 Persistence Databases to Support Workflow Versioning](using-workflowidentity-and-versioning.md#UpdatingWF4PersistenceDatabases).

## <a name="BKMK_NewActivities"></a> Activities

A biblioteca de atividades embutida contém novas atividades e novos recursos para atividades existentes.

### <a name="BKMK_NoPersistScope"></a> NoPersist Scope

<xref:System.Activities.Statements.NoPersistScope> é uma nova atividade do contêiner que impede que um fluxo de trabalho seja persistido quando as atividades filhos do NoPersistScope estiverem em execução. Isso é útil em cenários onde não é apropriado para o fluxo de trabalho ser persistido, como quando o fluxo de trabalho estiver usando recursos específicos de computadores como os identificadores de arquivos ou durante as transações de banco de dados. Anteriormente, para impedir que a persistência ocorresse durante a execução de uma atividade, um <xref:System.Activities.NativeActivity> personalizado que usava <xref:System.Activities.NoPersistHandle> era necessário.

### <a name="BKMK_NewFlowchartCapabilities"></a> New Flowchart Capabilities

Flowcharts are updated for .NET Framework 4.5 and have the following new capabilities:

- A propriedade `DisplayName` de uma atividade <xref:System.Activities.Statements.FlowSwitch%601> ou <xref:System.Activities.Statements.FlowDecision> é editável. Isso permitirá que o designer de atividade mostre mais informações sobre a finalidade da atividade.

- Os fluxogramas têm uma nova propriedade chamada <xref:System.Activities.Statements.Flowchart.ValidateUnconnectedNodes%2A>; o padrão para essa propriedade é `False`. Se essa propriedade for definida como `True`, os nós de fluxograma desconectados gerarão erros de validação.

## <a name="support-for-partial-trust"></a>Suporte para confiança parcial

Os fluxos de trabalho no .NET Framework 4 exigiam um domínio de aplicativo totalmente confiável. No .NET Framework 4,5, os fluxos de trabalho podem operar em um ambiente de confiança parcial. Em um ambiente de confiança parcial, os componentes de terceiros podem ser usados sem conceder a eles acesso completo aos recursos do host. Algumas preocupações sobre fluxos de trabalho em execução em confiança parcial são as seguintes:

1. Usar os componentes legados (incluindo regras) contidos na atividade <xref:System.Activities.Statements.Interop> não tem suporte em confiança parcial.

2. Fluxos de trabalho em execução em confiança parcial no <xref:System.ServiceModel.WorkflowServiceHost> não têm suporte.

3. Manter exceções em um cenário de confiança parcial é uma ameaça potencial de segurança. Para desabilitar a persistência de exceções, uma extensão do tipo <xref:System.Activities.ExceptionPersistenceExtension> deve ser adicionada ao projeto para deixar de persistir exceções. O exemplo de código a seguir demonstra como implementar esse tipo.

    ```csharp
    public class ExceptionPersistenceExtension
    {
        public ExceptionPersistenceExtension()
        {
            this.PersistExceptions = false;
        }
        public bool PersistExceptions { get; set; }
    }
    ```

     Se as exceções não tiverem que ser serializadas, verifique se as exceções são usadas dentro de um <xref:System.Activities.Statements.NoPersistScope>.

4. Os autores de atividade devem substituir <xref:System.Activities.Activity.CacheMetadata%2A> para evitar que o runtime de fluxo de trabalho execute automaticamente a reflexão em relação ao tipo. Os argumentos e as atividades filho devem ser não null e <xref:System.Activities.ActivityMetadata.Bind%2A> deve ser chamado explicitamente. Para obter mais informações sobre como substituir <xref:System.Activities.Activity.CacheMetadata%2A>, consulte [expondo dados com CacheMetadata](exposing-data-with-cachemetadata.md). Além disso, as instâncias de argumentos que são de um tipo `internal` ou **Private** devem ser explicitamente criadas em <xref:System.Activities.Activity.CacheMetadata%2A> para evitar serem criadas por reflexão.

5. Os tipos não usarão <xref:System.Runtime.Serialization.ISerializable> ou <xref:System.SerializableAttribute> para serialização; os tipos que tiverem que ser serializados devem oferecer suporte a <xref:System.Runtime.Serialization.DataContractSerializer>.

6. Expressões que usam <xref:System.Activities.Expressions.LambdaValue%601> exigem <xref:System.Security.Permissions.ReflectionPermissionAttribute.RestrictedMemberAccess%2A> e, dessa forma, não funcionarão em confiança parcial. Os fluxos de trabalho que usam <xref:System.Activities.Expressions.LambdaValue%601> devem substituir essas expressões pelas atividades que derivam de <xref:System.Activities.CodeActivity%601>. .

7. As expressões não podem ser criadas com <xref:System.Activities.XamlIntegration.TextExpressionCompiler> ou o compilador hospedado do Visual Basic em confiança parcial, mas as expressões criadas anteriormente podem ser executadas.

8. Um único assembly que usa a [transparência de nível 2](https://aka.ms/Level2Transparency) não pode ser usado no .NET Framework 4, [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] em confiança total e [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] em confiança parcial.

## <a name="BKMK_NewDesignerCapabilites"></a>Novos recursos do designer

### <a name="BKMK_DesignerSearch"></a>Pesquisa de designer

Para tornar os fluxos de trabalho maiores mais gerenciáveis, os fluxos de trabalho podem agora ser pesquisados por palavra-chave. Este recurso só está disponível no Visual Studio; Esse recurso não está disponível em um designer rehospedado. Há dois tipos de pesquisas disponível:

- Localização rápida, iniciada com **Ctrl + F** ou **Editar**, **Localizar e substituir**, **localização rápida**.

- Localizar em arquivos, iniciados com **Ctrl + Shift + F** ou **Editar**, **Localizar e substituir**, **Localizar em arquivos**.

Observe que Substituir não tem suporte.

#### <a name="BKMK_QuickFind"></a>Localização rápida

As palavras-chave pesquisadas em fluxos de trabalho corresponderão aos seguintes itens do designer:

- Propriedades de objetos <xref:System.Activities.Activity>, <xref:System.Activities.Statements.FlowNode>, <xref:System.Activities.Statements.State>, <xref:System.Activities.Statements.Transition> e outros itens de controle de fluxo personalizado.

- Variáveis

- Arguments

- Expressões

A Localização Rápida é executada na árvore do <xref:System.Activities.Presentation.Model.ModelItem> do designer. A Localização Rápida não localizará namespaces importados na definição de fluxo de trabalho.

#### <a name="BKMK_FindInFiles"></a>Localizar nos arquivos

As palavras-chave pesquisadas em fluxos de trabalho coincidirão com o conteúdo real dos arquivos de fluxo de trabalho. Os resultados da pesquisa serão mostrados no painel de exibição Localizar Resultados do Visual Studio. Clicar duas vezes no item de resultado navegará até a atividade que contém a correspondência no designer de fluxo de trabalho.

### <a name="BKMK_VariableDeleteContextMenu"></a>Excluir item de menu de contexto em Designer de argumento e de variável

No .NET Framework 4, variáveis e argumentos só podem ser excluídos no designer usando o teclado. A partir do .NET Framework 4,5, variáveis e argumentos podem ser excluídos usando o menu de contexto.

A captura de tela a seguir mostra o menu de contexto do designer de variável e argumento.

![Menu de contexto do designer de argumento e variável](./media/whats-new-in-wf-in-dotnet/designer-context-menu.png)

### <a name="BKMK_AutoSurround"></a>Surround automático com sequência

Como um fluxo de trabalho ou algumas atividades do contêiner (como <xref:System.Activities.Statements.NoPersistScope>) só podem conter uma única atividade do corpo, adicionar uma segunda atividade exigia que o desenvolvedor excluísse a primeira atividade, adicionasse uma atividade <xref:System.Activities.Statements.Sequence> e, em seguida, adicionasse as duas atividades para a atividade de sequência. A partir do .NET Framework 4,5, ao adicionar uma segunda atividade à superfície do designer, uma atividade de `Sequence` será criada automaticamente para encapsular as duas atividades.

A captura de tela a seguir mostra uma atividade de `WriteLine` no `Body` de um `NoPersistScope`.

![Uma atividade WriteLine no corpo de uma atividade NoPersistScope.](./media/whats-new-in-wf-in-dotnet/auto-surround-write-line-activity.png)

A captura de tela a seguir mostra a atividade de `Sequence` criada automaticamente no `Body` quando um segundo `WriteLine` é solto abaixo do primeiro.

![Uma sequência criada automaticamente no corpo de um NoPersistScope.](./media/whats-new-in-wf-in-dotnet/auto-surround-sequence-activity.png)

### <a name="BKMK_PanMode"></a>Modo panorâmico

Para navegar mais facilmente um grande fluxo de trabalho no designer, o modo panorâmico pode ser habilitado, permitindo que o desenvolvedor clique e arraste para mover a parte visível do fluxo de trabalho, em vez de precisar usar as barras de rolagem. O botão para ativar o modo de superfície está no canto inferior direito do designer.

A captura de tela a seguir mostra o botão de panorâmica qual está localizado no canto inferior direito do designer de fluxo de trabalho.

![O botão de panorâmica realçado no designer de fluxo de trabalho.](./media/whats-new-in-wf-in-dotnet/pan-button-workflow-designer.png)

O botão do meio do mouse ou a barra de espaço também podem ser usados para executar uma panorâmica do designer de fluxo de trabalho.

### <a name="BKMK_MultiSelect"></a>Seleção múltipla

Várias atividades podem ser selecionadas ao mesmo tempo, arrastando um retângulo ao redor delas (quando o modo panorâmico não está habilitado), ou mantendo pressionada a tecla CTRL e clicando nas atividades desejadas.

Várias seleções de atividade também podem ser arrastadas e soltadas dentro do designer, e também podem ser interagidas usando o menu de contexto.

### <a name="BKMK_DocumentOutline"></a>modo de exibição da Estrutura do Código de itens de fluxo de trabalho

Para facilitar a navegação de fluxos de trabalho hierárquicos, os componentes de um fluxo de trabalho são mostrados em uma exibição de destaque em estilo de árvore. O modo de exibição de estrutura de tópicos é exibido na exibição de **estrutura de tópicos do documento** . Para abrir essa exibição, no menu superior, selecione **exibição**, **outras janelas**, **estrutura de tópicos do documento**ou pressione CTRL +, U. Clicar em um nó na exibição de destaque navegará para a atividade correspondente no designer de fluxo de trabalho, e a exibição da estrutura será atualizada para mostrar as atividades que estão selecionadas no designer.

A seguinte captura de tela do fluxo de trabalho concluído do [tutorial de introdução](getting-started-tutorial.md) mostra o modo de exibição de estrutura de tópicos com um fluxo de trabalho Sequencial.

![Captura de tela da exibição de estrutura de tópicos com um fluxo de trabalho Sequencial no Visual Studio.](./media/whats-new-in-wf-in-dotnet/outline-view-in-workflow-designer.jpg)

### <a name="BKMK_CSharpExpressions"></a>C# Expressões

Antes do .NET Framework 4,5, todas as expressões em fluxos de trabalho só podiam ser gravadas em Visual Basic. No .NET Framework 4,5, Visual Basic expressões são usadas somente para projetos criados usando Visual Basic. Os projetos do Visual C# agora usam o C# para expressões. Um editor de expressão C# completamente funcional é fornecido com recursos como realce de gramática e intellisense. Os projetos do fluxo de trabalho C# criados em versões anteriores que usam expressões do Visual Basic continuarão funcionando.

As expressões C# são validadas em tempo de design. Os erros em expressões C# serão marcados com um sublinhado vermelho ondulado.

Para obter mais informações C# sobre expressões, consulte [ C# expressões](csharp-expressions.md).

### <a name="BKMK_Visibility"></a>Mais controle da visibilidade de itens de cabeçalho e barra do Shell

Em um designer hospedado novamente, alguns dos controles padrão de interface do usuário não podem ter o significado de um fluxo de trabalho específico, e podem ser desativados. No .NET Framework 4, essa personalização só é suportada pela barra de Shell na parte inferior do designer. No .NET Framework 4,5, a visibilidade dos itens de cabeçalho do Shell na parte superior do designer pode ser ajustada definindo <xref:System.Activities.Presentation.View.DesignerView.WorkflowShellHeaderItemsVisibility%2A> com o valor de <xref:System.Activities.Presentation.View.ShellHeaderItemsVisibility> apropriado.

### <a name="BKMK_AutoConnect"></a>Conexão automática e inserção automática em fluxos de trabalho do fluxograma e da máquina de estado

No .NET Framework 4, as conexões entre os nós em um fluxo de trabalho de fluxograma tinham que ser adicionadas manualmente. No .NET Framework 4,5, os nós de máquina de estado e fluxograma têm pontos de conexão automática que se tornam visíveis quando uma atividade é arrastada da caixa de ferramentas para a superfície do designer. Soltar uma atividade em um destes pontos adiciona automaticamente a atividade junto com a conexão necessária.

A captura de tela a seguir mostra os pontos de anexação que ficam visíveis quando uma atividade é arrastada da caixa de ferramentas.

![Nó de início do fluxograma mostrando pontos de conexão automática](./media/whats-new-in-wf-in-dotnet/auto-connect-points-start-node.png)

As atividades também podem ser arrastadas em conexões entre nós de fluxograma e estados para inserção automática do nó entre dois outros nós. A captura de tela a seguir mostra a linha de conexão realçada em que as atividades podem ser arrastadas da caixa de ferramentas e soltas.

![Identificador de inserção automática para soltar atividades](./media/whats-new-in-wf-in-dotnet/auto-insert-connecting-line.png)

### <a name="BKMK_Annotations"></a>Anotações do designer

Para facilitar o desenvolvimento de fluxos de trabalho maiores, o designer agora permite adicionar anotações para ajudar a controlar o processo de design. A anotação pode ser adicionada a atividades, estados, nós de fluxograma, variáveis e argumentos. A captura de tela a seguir mostra o menu de contexto usado para adicionar anotações para o designer.

![Captura de tela mostrando um menu para adicionar anotações.](./media/whats-new-in-wf-in-dotnet/designer-annotations-context-menu.png)

### <a name="debugging-states"></a>Estados de depuração

No .NET Framework 4, os elementos que não são de atividade não podiam dar suporte a pontos de interrupção de depuração, pois não eram unidades de execução. Esta versão fornece um mecanismo para adicionar pontos de interrupção a objetos <xref:System.Activities.Statements.State>. Quando um ponto de interrupção for definido em um <xref:System.Activities.Statements.State>, a execução será interrompida quando ocorrer a transição do estado antes que suas atividades de entrada ou gatilhos sejam agendados.

### <a name="BKMK_ActivityDelegates"></a>Definir e consumir objetos ActivityDelegate no designer

Activities in .NET Framework 4 used <xref:System.Activities.ActivityDelegate> objects to expose execution points where other parts of the workflow could interact with a workflow's execution, but using these execution points usually required a fair amount of code. Nesta versão, os desenvolvedores podem definir e consumir representantes de atividade usando o designer de fluxo de trabalho. For more information, see [How to: Define and consume activity delegates in the Workflow Designer](/visualstudio/workflow-designer/how-to-define-and-consume-activity-delegates-in-the-workflow-designer).

### <a name="BKMK_BuildTimeValidation"></a> Build-time validation

In .NET Framework 4, workflow validation errors weren’t counted as build errors during the build of a workflow project. Isso significava que criar um projeto de fluxo de trabalho poderia ter êxito mesmo se houvesse erros de validação do fluxo de trabalho. In .NET Framework 4.5, workflow validation errors cause the build to fail.

### <a name="BKMK_DesignTimeValidation"></a> Design-time background validation

In .NET Framework 4, workflows were validated as a foreground process, which could potentially block the UI during complex or time-consuming validation processes. A validação do fluxo de trabalho agora ocorre em um thread em segundo plano, de modo que a interface do usuário não seja bloqueada.

### <a name="BKMK_ViewState"></a> View state located in a separate location in XAML files

In .NET Framework 4, the view state information for a workflow is stored across the XAML file in many different locations. Isso é inconveniente para os desenvolvedores que desejam ler XAML diretamente, ou gravar código para remover informações de estado de exibição. In .NET Framework 4.5, the view state information in the XAML file is serialized as a separate element in the XAML file. Developers can easily locate and edit the view state information of an activity, or remove the view state altogether.

### <a name="BKMK_ExpressionExtensibility"></a> Expression extensibility

In .NET Framework 4.5, we provide a way for developers to create their own expression and expression authoring experience that can be plugged into the workflow designer.

### <a name="BKMK_BackwardCompatRehostedDesigner"></a> Opt-in for Workflow 4.5 features in rehosted designer

To preserve backward compatibility, some new features included in .NET Framework 4.5 are not enabled by default in the rehosted designer. Este é para garantir que aplicativos existentes que usam o designer hospedado novamente não sejam interrompidos ao atualizar para a versão mais recente. Para habilitar novos recursos no designer hospedado novamente, defina <xref:System.Activities.Presentation.DesignerConfigurationService.TargetFrameworkName%2A> como ".NET Framework 4.5" ou defina membros individuais do conjunto de <xref:System.Activities.Presentation.DesignerConfigurationService> para habilitar recursos individuais.

## <a name="BKMK_NewWFModels"></a> New Workflow Development Models

Além do fluxograma e de modelos sequenciais de desenvolvimento de fluxo de trabalho, esta versão inclui fluxos de trabalho da Máquina de Estado e serviços de fluxo de trabalho de primeiro contrato.

### <a name="BKMK_StateMachine"></a> State machine workflows

State machine workflows were introduced as part of the .NET Framework 4, version 4.0.1 in the [Microsoft .NET Framework 4 Platform Update 1](https://go.microsoft.com/fwlink/?LinkID=215092). Essa atualização incluiu várias novas classes e atividades que permitiram que os desenvolvedores criassem fluxos de trabalho de máquina do estado. These classes and activities have been updated for .NET Framework 4.5. As atualizações incluem:

1. A capacidade de definir pontos de interrupção em estados

2. A capacidade de copiar e colar transições no designer de fluxo de trabalho

3. Suporte de designer para criação de transição do disparador compartilhado

4. Atividades usadas para criar fluxos de trabalho da máquina de estado, incluindo: <xref:System.Activities.Statements.StateMachine>, <xref:System.Activities.Statements.State> e <xref:System.Activities.Statements.Transition>.

The following screenshot shows the completed state machine workflow from the [Getting Started Tutorial](getting-started-tutorial.md) step [How to: Create a State Machine Workflow](how-to-create-a-state-machine-workflow.md).

![Illustration that shows the completed state machine workflow.](./media/whats-new-in-wf-in-dotnet/complete-state-machine-workflow.jpg)

For more information on creating state machine workflows, see [State Machine Workflows](state-machine-workflows.md).

### <a name="BKMK_ContractFirst"></a> Contract-first workflow development

The contract-first workflow development tool allows the developer to design a contract in code first, then, with a few clicks in Visual Studio, automatically generate an activity template in the toolbox representing each operation. Essas atividades são então usadas para criar um fluxo de trabalho que implementa as operações definidas pelo contrato. O designer de fluxo de trabalho validará o serviço de fluxo de trabalho para assegurar que essas operações sejam implementadas e a assinatura do fluxo de trabalho corresponda à assinatura do contrato. O desenvolvedor também pode associar um serviço de fluxo de trabalho a uma coleção de contratos implementados. For more information on contract-first workflow service development, see [How to: Create a workflow service that consumes an existing service contract](how-to-create-a-workflow-service-that-consumes-an-existing-service-contract.md).
