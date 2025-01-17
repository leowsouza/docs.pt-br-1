---
title: 'Tutorial: analisar o sentimentos das análises de filmes usando um modelo de TensorFlow pré-treinado'
description: Este tutorial mostra como usar um modelo de TensorFlow pré-treinado para classificar as opiniões nos comentários do site. O classificador de sentimentos binário C# é um aplicativo de console desenvolvido usando o Visual Studio.
ms.date: 11/15/2019
ms.topic: tutorial
ms.custom: mvc
ms.author: nakersha
author: natke
ms.openlocfilehash: 8c3544b60b1fba1d419ca091b0a1d85fbbdbe2d6
ms.sourcegitcommit: 81ad1f09b93f3b3e6706a7f2e4ddf50ef229ea3d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74204929"
---
# <a name="tutorial-analyze-sentiment-of-movie-reviews-using-a-pre-trained-tensorflow-model-in-mlnet"></a>Tutorial: analisar o sentimentos das análises de filmes usando um modelo de TensorFlow pré-treinado no ML.NET

Este tutorial mostra como usar um modelo de TensorFlow pré-treinado para classificar as opiniões nos comentários do site. O classificador de sentimentos binário C# é um aplicativo de console desenvolvido usando o Visual Studio.

O modelo TensorFlow usado neste tutorial foi treinado usando revisões de filme do banco de dados do IMDB. Depois de concluir o desenvolvimento do aplicativo, você poderá fornecer o texto de revisão do filme e o aplicativo informará se a revisão tem uma opinião positiva ou negativa.

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
>
> * Carregar um modelo de TensorFlow pré-treinado
> * Transformar o texto de comentário do site em recursos adequados para o modelo
> * Usar o modelo para fazer uma previsão

Você pode encontrar o código-fonte para este tutorial no repositório [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TextClassificationTF).

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio 2017 versão 15,6 ou posterior](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) com a carga de trabalho "desenvolvimento de plataforma cruzada do .NET Core" instalada.

## <a name="setup"></a>Configuração

### <a name="create-the-application"></a>Criar o aplicativo

1. Crie um **aplicativo de console do .NET Core** chamado "TextClassificationTF".

2. Crie um diretório chamado *Data* no seu projeto para salvar os arquivos do conjunto de dados.

3. Instalar o **Pacote NuGet Microsoft.ML**:

    No Gerenciador de Soluções, clique com o botão direito do mouse no seu projeto e selecione **Gerenciar Pacotes NuGet**. Escolha "nuget.org" como a origem do pacote e, em seguida, selecione a guia **procurar** . procure **Microsoft.ml**, selecione o pacote desejado e, em seguida, selecione o botão **instalar** . Prossiga com a instalação concordando com os termos de licença do pacote que você escolher. Repita essas etapas para **Microsoft. ml. TensorFlow** e **SciSharp. TensorFlow. Redist**.

### <a name="add-the-tensorflow-model-to-the-project"></a>Adicionar o modelo TensorFlow ao projeto

> [!NOTE]
> O modelo para este tutorial é do repositório GitHub [dotnet/MachineLearning-TestData](https://github.com/dotnet/machinelearning-testdata/tree/master/Microsoft.ML.TensorFlow.TestModels/sentiment_model) . O modelo está no formato TensorFlow SavedModel.

1. Baixe o [arquivo zip sentiment_model](https://github.com/dotnet/samples/blob/master/machine-learning/models/textclassificationtf/sentiment_model.zip?raw=true)e descompacte.

    O arquivo zip contém:

    * `saved_model.pb`: o próprio modelo de TensorFlow. O modelo usa uma matriz de inteiros de tamanho fixo (tamanho 600) de recursos que representam o texto em uma cadeia de caracteres de revisão do IMDB e gera duas probabilidades que somam 1: a probabilidade de que a revisão de entrada tenha uma opinião positiva e a probabilidade de que a revisão de entrada tenha sentimentos negativo.
    * `imdb_word_index.csv`: um mapeamento de palavras individuais para um valor inteiro. O mapeamento é usado para gerar os recursos de entrada para o modelo TensorFlow.

2. Copie o conteúdo do diretório `sentiment_model` mais interno para o seu projeto do *TextClassificationTF* `sentiment_model` diretório. Este diretório contém o modelo e os arquivos de suporte adicionais necessários para este tutorial, conforme mostrado na imagem a seguir:

   ![conteúdo do sentiment_model Directory](./media/text-classification-tf/sentiment-model-files.png)

3. Em Gerenciador de Soluções, clique com o botão direito do mouse em cada um dos arquivos no diretório `sentiment_model` e no subdiretório e selecione **Propriedades**. Em **Avançado**, altere o valor de **Copiar para Diretório de Saída** para **Copiar se for mais novo**.

### <a name="add-using-statements-and-global-variables"></a>Adicionar instruções using e variáveis globais

1. Adicione as seguintes instruções `using` adicionais ao início do arquivo *Program.cs*:

   [!code-csharp[AddUsings](../../../samples/machine-learning/tutorials/TextClassificationTF/Program.cs#AddUsings "Add necessary usings")]

1. Crie duas variáveis globais logo acima do método `Main` para manter o caminho do arquivo de modelo salvo e o comprimento do vetor de recurso.

   [!code-csharp[DeclareGlobalVariables](../../../samples/machine-learning/tutorials/TextClassificationTF/Program.cs#DeclareGlobalVariables "Declare global variables")]

    * `_modelPath` é o caminho do arquivo do modelo treinado.
    * `FeatureLength` é o comprimento da matriz de recursos de inteiro que o modelo espera.

### <a name="model-the-data"></a>Modelar os dados

As revisões de filme são texto de forma livre. Seu aplicativo converte o texto no formato de entrada esperado pelo modelo em uma série de estágios discretos.

A primeira é dividir o texto em palavras separadas e usar o arquivo de mapeamento fornecido para mapear cada palavra em uma codificação de número inteiro. O resultado dessa transformação é uma matriz de inteiro de comprimento variável com um comprimento correspondente ao número de palavras na sentença.

|Propriedade| Valor|Tipo|
|-------------|-----------------------|------|
|ReviewText|Esse filme é realmente bom|cadeia de caracteres|
|VariableLengthFeatures|14, 22, 9, 66, 78,... |int []|

Em seguida, a matriz de recursos de comprimento variável é redimensionada para um comprimento fixo de 600. Esse é o comprimento esperado pelo modelo TensorFlow.

|Propriedade| Valor|Tipo|
|-------------|-----------------------|------|
|ReviewText|Esse filme é realmente bom|cadeia de caracteres|
|VariableLengthFeatures|14, 22, 9, 66, 78,... |int []|
|Recursos|14, 22, 9, 66, 78,... |int [600]|

1. Crie uma classe para os dados de entrada, após o método de `Main`:

    [!code-csharp[MovieReviewClass](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#MovieReviewClass "Declare movie review type")]

    A classe de dados de entrada, `MovieReview`, tem um `string` para comentários do usuário (`ReviewText`).

1. Crie uma classe para os recursos de comprimento variável, após o método de `Main`:

    [!code-csharp[VariableLengthFeatures](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#VariableLengthFeatures "Declare variable length features type")]

    A propriedade `VariableLengthFeatures` tem um atributo [vectortype](xref:Microsoft.ML.Data.VectorTypeAttribute.%23ctor%2A) para designá-la como um vetor.  Todos os elementos de vetor devem ser do mesmo tipo. Em conjuntos de dados com um grande número de colunas, carregar várias colunas como um único vetor reduz o número de passagens de dados quando você aplica transformações de dados.

    Essa classe é usada na ação `ResizeFeatures`. Os nomes de suas propriedades (neste caso, apenas um) são usados para indicar quais colunas no DataView podem ser usadas como _entrada_ para a ação de mapeamento personalizado.

1. Crie uma classe para os recursos de comprimento fixo, após o método de `Main`:

    [!code-csharp[FixedLengthFeatures](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#FixedLengthFeatures)]

    Essa classe é usada na ação `ResizeFeatures`. Os nomes de suas propriedades (neste caso, apenas um) são usados para indicar quais colunas no DataView podem ser usadas como a _saída_ da ação de mapeamento personalizado.

    Observe que o nome da propriedade `Features` é determinado pelo modelo TensorFlow. Você não pode alterar esse nome de propriedade.

1. Crie uma classe para a previsão após o método de `Main`:

    [!code-csharp[Prediction](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#Prediction "Declare prediction class")]

    `MovieReviewSentimentPrediction` é a classe de previsão usada após o treinamento do modelo. `MovieReviewSentimentPrediction` tem uma única matriz de `float` (`Prediction`) e um atributo de `VectorType`.

### <a name="create-the-mlcontext-lookup-dictionary-and-action-to-resize-features"></a>Criar o MLContext, o dicionário de pesquisa e a ação para redimensionar recursos

A [classe MLContext](xref:Microsoft.ML.MLContext) é um ponto de partida para todas as operações do ML.NET. Inicializar `mlContext` cria um novo ambiente do ML.NET que pode ser compartilhado entre os objetos do fluxo de trabalho de criação de modelo. Ele é semelhante, conceitualmente, a `DBContext` no Entity Framework.

1. Substitua a linha `Console.WriteLine("Hello World!")` no método `Main` pelo seguinte código para declarar e inicializar a variável mlContext:

   [!code-csharp[CreateMLContext](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#CreateMLContext "Create the ML Context")]

1. Crie um dicionário para codificar palavras como inteiros usando o método [`LoadFromTextFile`](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A) para carregar dados de mapeamento de um arquivo, como mostrado na tabela a seguir:

    |Palavra     |Índice    |
    |---------|---------|
    |Kids     |  362    |
    |deseja     |  181    |
    |errado    |  355    |
    |efeitos  |  302    |
    |sensação  |  547    |

    Adicione o código abaixo para criar o mapa de pesquisa:

    [!code-csharp[CreateLookupMap](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#CreateLookupMap)]

1. Adicione um `Action` para redimensionar o tamanho da variável matriz de inteiros da palavra para uma matriz de tamanho inteiro fixo, com as próximas linhas de código:

   [!code-csharp[ResizeFeatures](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#ResizeFeatures)]

## <a name="load-the-pre-trained-tensorflow-model"></a>Carregar o modelo de TensorFlow pré-treinado

1. Adicione o código para carregar o modelo TensorFlow:

    [!code-csharp[LoadTensorFlowModel](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#LoadTensorFlowModel)]

    Depois que o modelo for carregado, você poderá extrair seu esquema de entrada e saída. Os esquemas são exibidos apenas para fins de interesse e aprendizado. Você não precisa desse código para que o aplicativo final funcione:

    [!code-csharp[GetModelSchema](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#GetModelSchema)]

    O esquema de entrada é a matriz de comprimento fixo de palavras codificadas por inteiro. O esquema de saída é uma matriz float de probabilidades que indica se a opinião de uma revisão é negativa ou positiva. Esses valores somam 1, uma vez que a probabilidade de ser positiva é o complemento da probabilidade de que o sentimentos seja negativo.

## <a name="create-the-mlnet-pipeline"></a>Criar o pipeline ML.NET

1. Crie o pipeline e divida o texto de entrada em palavras usando [TokenizeIntoWords](xref:Microsoft.ML.TextCatalog.TokenizeIntoWords%2A) Transform para quebrar o texto em palavras como a próxima linha de código:

   [!code-csharp[TokenizeIntoWords](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#TokenizeIntoWords)]

   A transformação [TokenizeIntoWords](xref:Microsoft.ML.TextCatalog.TokenizeIntoWords%2A) usa espaços para analisar o texto/cadeia de caracteres em palavras. Ele cria uma nova coluna e divide cada cadeia de caracteres de entrada em um vetor de subcadeias de caracteres com base no separador definido pelo usuário.

1. Mapeie as palavras em sua codificação de número inteiro usando a tabela de pesquisa declarada acima:

    [!code-csharp[MapValue](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#MapValue)]

1. Redimensione as codificações de inteiro de comprimento variável para o comprimento fixo exigido pelo modelo:

    [!code-csharp[CustomMapping](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#CustomMapping)]

1. Classifique a entrada com o modelo TensorFlow carregado:

    [!code-csharp[ScoreTensorFlowModel](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#ScoreTensorFlowModel)]

    A saída do modelo TensorFlow é chamada de `Prediction/Softmax`. Observe que o nome `Prediction/Softmax` é determinado pelo modelo TensorFlow. Você não pode alterar esse nome.

1. Crie uma nova coluna para a previsão de saída:

    [!code-csharp[SnippetCopyColumns](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#SnippetCopyColumns)]

    Você precisa copiar a coluna `Prediction/Softmax` em uma com um nome que possa ser usado como uma propriedade em uma C# classe: `Prediction`. O caractere de `/` não é permitido em C# um nome de propriedade.

## <a name="create-the-mlnet-model-from-the-pipeline"></a>Criar o modelo ML.NET a partir do pipeline

1. Adicione o código para criar o modelo a partir do pipeline:

    [!code-csharp[SnippetCreateModel](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#SnippetCreateModel)]

    Um modelo ML.NET é criado a partir da cadeia de estimadores no pipeline chamando o método `Fit`. Nesse caso, não estamos ajustando nenhum dado para criar o modelo, pois o modelo TensorFlow já foi treinado anteriormente. Fornecemos um objeto de exibição de dados vazio para atender aos requisitos do método `Fit`.

## <a name="use-the-model-to-make-a-prediction"></a>Usar o modelo para fazer uma previsão

1. Adicione o método `PredictSentiment` abaixo do método `Main`:

    ```csharp
    public static void PredictSentiment(MLContext mlContext, ITransformer model)
    {

    }
    ```

1. Adicione o seguinte código para criar o `PredictionEngine` como a primeira linha no método `PredictSentiment()`:

    [!code-csharp[CreatePredictionEngine](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#CreatePredictionEngine)]

    O [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) é uma API de conveniência, que permite que você execute uma previsão em uma única instância de dados. [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) não é thread-safe. É aceitável usar em ambientes de protótipo ou de thread único. Para melhorar o desempenho e a segurança de thread em ambientes de produção, use o serviço `PredictionEnginePool`, que cria uma [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) de objetos [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) para uso em todo o aplicativo. Consulte este guia sobre como [usar `PredictionEnginePool` em uma API Web do ASP.NET Core](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application).

    > [!NOTE]
    > A extensão de serviço `PredictionEnginePool` está atualmente em versão prévia.

1. Adicione um comentário para testar as previsões do modelo treinado no método `Predict()` ao criar uma instância de `MovieReview`:

    [!code-csharp[CreateTestData](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#CreateTestData)]

1. Passe os dados de comentário de teste para a `Prediction Engine` adicionando as próximas linhas de código no método `PredictSentiment()`:

    [!code-csharp[Predict](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#Predict)]

1. A função [Predict ()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) faz uma previsão em uma única linha de dados:

    |Propriedade| Valor|Tipo|
    |-------------|-----------------------|------|
    |Previsão|[0,5459937, 0,454006255]|float []|

1. Exiba a previsão de sentimentos usando o seguinte código:

    [!code-csharp[DisplayPredictions](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#DisplayPredictions)]

1. Adicione uma chamada para `PredictSentiment` no final do método `Main`:

    [!code-csharp[CallPredictSentiment](~/samples/machine-learning/tutorials/TextClassificationTF/Program.cs#CallPredictSentiment)]

## <a name="results"></a>Resultados

Compile e execute seu aplicativo.

Seus resultados devem ser semelhantes aos seguintes. Durante o processamento, as mensagens são exibidas. Você pode ver avisos ou mensagens de processamento. Essas mensagens foram removidas dos resultados a seguir para ficar mais claro.

```console
Number of classes: 2
Is sentiment/review positive ? Yes
```

Parabéns! Agora você criou com êxito um modelo de aprendizado de máquina para classificar e prever os sentimentos de mensagens reutilizando um modelo de `TensorFlow` pré-treinado em ML.NET.

Você pode encontrar o código-fonte para este tutorial no repositório [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/TextClassificationTF).

Neste tutorial, você aprendeu como:
> [!div class="checklist"]
>
> * Carregar um modelo de TensorFlow pré-treinado
> * Transformar o texto de comentário do site em recursos adequados para o modelo
> * Usar o modelo para fazer uma previsão
