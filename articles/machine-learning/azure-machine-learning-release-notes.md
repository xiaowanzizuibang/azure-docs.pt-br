---
title: Novidades na versão
titleSuffix: Azure Machine Learning
description: Saiba mais sobre as atualizações mais recentes para Azure Machine Learning e o aprendizado de máquina e SDKs do Python de preparação de dados.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: jmartens
author: j-martens
ms.date: 03/10/2020
ms.openlocfilehash: eaf94bcc894a7ac4d05540239695094856ed8a41
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82186962"
---
# <a name="azure-machine-learning-release-notes"></a>Notas de versão do Azure Machine Learning

Neste artigo, saiba mais sobre as versões de Azure Machine Learning.  Para obter o conteúdo completo de referência do SDK, visite a página de referência do [**SDK principal do Azure Machine Learning para Python**](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) .

Veja [a lista de problemas conhecidos](resource-known-issues.md) para aprender sobre erros e soluções conhecidas.

## <a name="2020-04-27"></a>2020-04-27

### <a name="azure-machine-learning-sdk-for-python-v140"></a>Azure Machine Learning SDK para Python v 1.4.0

+ **Novos recursos**
  + Os clusters AmlCompute agora dão suporte à configuração de uma identidade gerenciada no cluster no momento do provisionamento. Basta especificar se você deseja usar uma identidade atribuída pelo sistema ou uma identidade atribuída pelo usuário e passar uma identityid no caso do último. Em seguida, você pode configurar o permissiosn para acessar vários recursos, como armazenamento ou ACR, de uma forma que a identidade da computação seja usada para acessar os dados com segurança, em vez de uma abordagem baseada em token que o AmlCompute emprega hoje. Confira nossa referência de SDK para obter mais informações sobre os parâmetros.
  

+ **Alterações de quebra**
  + Os clusters AmlCompute suportavam um recurso de visualização em relação à criação baseada em execução, que estamos planejando para substituir em duas semanas. Você pode continuar criando destinos de computação persistentes como sempre usando a classe Amlcompute, mas a abordagem específica de especificar o identificador "Amlcompute" como o destino de computação na configuração de execução não terá suporte em um futuro próximo. 

+ **Correções de bugs e melhorias**
  + **azureml-automl-tempo de execução**
    + Habilite o suporte para tipo sem hash ao calcular o número de valores exclusivos em uma coluna.
  + **azureml-núcleo**
    + Estabilidade aprimorada ao ler do armazenamento de BLOBs do Azure usando um TabularDataset.
    + Documentação aprimorada para `grant_workspace_msi` o parâmetro `Datastore.register_azure_blob_store`para.
    + Corrigido o bug `datastore.upload` com onde, `src_dir` se o argumento terminar `/` com `\`um ou, não será possível carregá-lo.
    + Mensagem de erro acionável adicionada ao tentar carregar em um armazenamento de blob do Azure que não tem uma chave de acesso ou token SAS.
  + **azureml-interpret**
    + Adição do limite superior ao tamanho do arquivo para os dados de visualização nas explicações carregadas.
  + **azureml-train-automl-client**
    + Verificando explicitamente label_column_name & parâmetros weight_column_name para que AutoMLConfig seja do tipo cadeia de caracteres.
  + **azureml-contrib-pipeline-etapas**
    + ParallelRunStep agora dá suporte a DataSet como parâmetro de pipeline. O usuário pode construir o pipeline com o conjunto de dados de exemplo e pode alterar o conjunto de dados de entrada do mesmo tipo (arquivo ou tabela) para a nova execução do pipeline.

  
## <a name="2020-04-13"></a>2020-04-13

### <a name="azure-machine-learning-sdk-for-python-v130"></a>Azure Machine Learning SDK para Python v 1.3.0

+ **Correções de bugs e melhorias**
  + **azureml-automl-core**
    + Adição de telemetria adicional em operações de pós-treinamento.
    + Acelera o treinamento de ARIMA automático usando o treinamento de soma de quadrados (CSS) condicional para uma série de comprimento maior que 100. Observe que o comprimento usado é armazenado como a constante ARIMA_TRIGGER_CSS_TRAINING_LENGTH w/na classe TimeSeriesInternal em/src/azureml-automl-Core/azureml/automl/Core/Shared/Constants.py
    + O registro em log de execuções de previsão foi aprimorado. agora, mais informações sobre qual fase está em execução no momento serão mostradas no log
    + Target_rolling_window_size não permitido ser definido como valores menores depois 2
  + **azureml-automl-tempo de execução**
    + Mensagem de erro mostrada quando são encontrados carimbos de data/hora duplicados.
    + Target_rolling_window_size não permitido ser definido como valores menos de 2.
    + Correção da falha de imputação de latência. O problema foi causado pelo número insuficiente de observações necessárias para decompor uma série em um época. Os dados "de-sazonal" são usados para calcular uma função parcial de correlação (PACF) para determinar o comprimento de latência.
    + Habilitada a personalização de personalização de finalidade de coluna para prever tarefas por personalização config. Os valores numéricos e categóricos como finalidade de coluna para tarefas de previsão agora têm suporte.
    + Habilite a personalização de personalização de coluna para previsão de tarefas por personalização config.
    + Habilitada a personalização de imputação para tarefas de previsão pela configuração do personalização. O valor constante imputação para a coluna de destino e média, mediana, most_frequent e o valor constante imputação para dados de treinamento agora têm suporte.
  + **azureml-contrib-pipeline-etapas**
    + Aceite os nomes de computação da cadeia de caracteres a serem passados para ParallelRunConfig
  + **azureml-núcleo**
    +  API de Environment. Clone (new_name) adicionada para criar uma cópia do objeto de ambiente
    +  Ambiente. Docker. base_dockerfile aceita FilePath. Se for capaz de resolver um arquivo, o conteúdo será lido na propriedade de ambiente base_dockerfile
    + Redefine automaticamente valores mutuamente exclusivos para base_image e base_dockerfile quando o usuário define manualmente um valor no ambiente. Docker
    + Foi adicionado user_managed sinalizador em RSection que indica se o ambiente é gerenciado pelo usuário ou pelo AzureML.
    + DataSet: falha no download do conjunto de dados fixo se o caminho de dado que contém caracteres Unicode.
    + Conjunto de valores: mecanismo de cache de montagem de conjunto de valores aprimorado para respeitar o requisito mínimo de espaço em disco no Azure Machine Learning computação, o que evita tornar o nó inutilizável e fazer com que o trabalho seja cancelado.
    + Conjunto de dados: adicionamos um índice para a coluna timeseries quando você acessa um conjunto de dados da série time, como um data frames do pandas, que é usado para acelerar o acesso ao acesso a dados com base na série temporal.  Anteriormente, o índice recebeu o mesmo nome que a coluna timestamp, os usuários confusos sobre qual é a coluna de carimbo de data/hora real e qual é o índice. Agora, não damos nenhum nome específico ao índice, pois ele não deve ser usado como uma coluna. 
    + DataSet: problema de autenticação de conjunto de banco de uma correção no soberanas Cloud.
    + Conjunto de valores `Dataset.to_spark_dataframe` : falha fixa para conjuntos de itens criados a partir de repositórios de armazenamento do PostgreSQL do Azure.
  + **azureml-interpret**
    + Foram adicionadas pontuações globais à visualização se os valores de importância local forem esparsos
    + Atualizado o azureml-interpretar para usar a interpretação-Community 0,9. *
    + Correção do problema com a explicação de download que tinha dados de avaliação esparsas
    + Adicionado suporte do formato esparso do objeto de explicação em AutoML
  + **azureml-pipeline-Core**
    + Suporte a ComputeInstance como destino de computação em pipelines
  + **azureml-train-automl-client**
    + Adição de telemetria adicional em operações de pós-treinamento.
    + Correção da regressão na parada inicial
    + O azureml. dprep. Dataflow foi preterido como um tipo válido para dados de entrada.
    +  Alterar o tempo limite de experimento do AutoML padrão para 6 dias.
  + **azureml-Train-automl-Runtime**
    + Adição de telemetria adicional em operações de pós-treinamento.
    + Adicionado suporte E2E automl esparso
  + **azureml-opendatasets**
    + Telemetria adicional adicionada para o monitor de serviço.
    + Habilitar frontdoor para blob para aumentar a estabilidade 

## <a name="2020-03-23"></a>2020-03-23

### <a name="azure-machine-learning-sdk-for-python-v120"></a>Azure Machine Learning SDK para Python v 1.2.0

+ **Alterações de quebra**
  + Descartar suporte para Python 2,7

+ **Correções de bugs e melhorias**
  + **azure-cli-ml**
    + Adiciona "--Subscription-ID" aos `az ml model/computetarget/service` comandos na CLI
    + Adição de suporte para passar CMK (chave gerenciada pelo cliente) vault_url, key_name e key_version para implantação do ACI
  + **azureml-automl-core** 
    + Habilitada a imputação personalizada com valor constante para tarefas de previsão de dados X e y.
    + Correção do problema no com a exibição de mensagens de erro para o usuário.    
  + **azureml-automl-tempo de execução**
    + Correção do problema no com a previsão nos conjuntos de dados, contendo refinamentos com apenas uma linha
    + Diminuiu a quantidade de memória exigida pelas tarefas de previsão.
    + Adicionadas melhores mensagens de erro se a coluna de tempo tiver um formato incorreto.
    + Habilitada a imputação personalizada com valor constante para tarefas de previsão de dados X e y.
  + **azureml-núcleo**
    + Adicionado suporte para carregar o servicePrincipalName de variáveis de ambiente: AZUREML_SERVICE_PRINCIPAL_ID, AZUREML_SERVICE_PRINCIPAL_TENANT_ID e AZUREML_SERVICE_PRINCIPAL_PASSWORD
    + Introduziu um novo `support_multi_line` parâmetro `Dataset.Tabular.from_delimited_files`para: por padrão`support_multi_line=False`(), todas as quebras de linha, incluindo aquelas em valores de campo entre aspas, serão interpretadas como uma quebra de registro. A leitura de dados dessa maneira é mais rápida e mais otimizada para execução paralela em vários núcleos de CPU. No entanto, isso pode resultar na produção silenciosa de mais registros com valores de campo desalinhados. Isso deve ser definido como `True` quando os arquivos delimitados são conhecidos por conter quebras de linha entre aspas.
    + Adicionada a capacidade de registrar ADLS Gen2 na CLI do Azure Machine Learning
    + Parâmetro renomeado ' fine_grain_timestamp ' para ' timestamp ' e parâmetro ' coarse_grain_timestamp ' como ' partition_timestamp ' para o método with_timestamp_columns () em TabularDataset para refletir melhor o uso dos parâmetros.
    + Tamanho máximo do nome de experimento aumentado para 255.
  + **azureml-interpret**
    + Atualizado o azureml-interpretar para interpretar-Community 0,7. *
  + **azureml-SDK**
    + Mudando para dependências com til de versão compatível para o suporte de aplicação de patch em versões de pré-lançamento e estáveis.


## <a name="2020-03-11"></a>2020-03-11

### <a name="azure-machine-learning-sdk-for-python-v115"></a>Azure Machine Learning SDK para Python v 1.1.5

+ **Substituição de recursos**
  + **Python 2,7**
    + Última versão para dar suporte ao Python 2,7

+ **Alterações de quebra**
  + **Controle de Versão Semântico 2.0.0**
    + A partir da versão 1,1, o SDK do Python do Azure ML adota o controle de versão semântico 2.0.0. [Leia mais aqui](https://semver.org/). Todas as versões subsequentes seguirão o novo esquema de numeração e o contrato de controle de versão semântico. 

+ **Correções de bugs e melhorias**
  + **azure-cli-ml**
    + Altere o nome do comando CLI do ponto de extremidade de ' AZ ml Endpoint AKs ' para ' AZ ml Endpoint realtime ' para fins de consistência.
    + atualizar instruções de instalação da CLI para a CLI do Branch estável e experimental
    + A criação de perfil de instância única foi corrigida para produzir uma recomendação e foi disponibilizada no SDK principal.
  + **azureml-automl-core**
    + Habilitada a inferência de modo de lote (levando várias linhas uma vez) para modelos automl ONNX
    + Melhoria da detecção de frequência nos conjuntos de dados, falta de dados ou contém pontos de dados irregulares
    + Adicionada a capacidade de remover pontos de dados que não estão em conformidade com a frequência dominante.
    + Alteração da entrada do construtor para obter uma lista de opções para aplicar as opções imputação para as colunas correspondentes.
    + O log de erros foi aprimorado.
  + **azureml-automl-tempo de execução**
    + Corrigido o problema com o erro gerado se o detalhamento que não estava presente no conjunto de treinamento apareceu no conjunto de testes
    + Removida a y_query requisito durante a pontuação no serviço de previsão
    + Corrigido o problema com a previsão quando o conjunto de dados contém granulações curtas com intervalos de tempo longos.
    + Corrigido o problema quando o horizonte máximo automático é ativado e a coluna data contém datas na forma de cadeias de caracteres. A conversão adequada e as mensagens de erro foram adicionadas para quando a conversão em data não é possível
    + Usando NumPy e SciPy nativos para serialização e desserialização de dados intermediários para FileCacheStore (usado para execuções locais de AutoML)
    + Corrigido um bug em que as execuções de filho com falha poderiam ficar paralisadas no estado de execução.
    + Maior velocidade de personalização.
    + Correção da verificação de frequência durante a pontuação, agora as tarefas de previsão não exigem uma equivalência de frequência estrita entre o treinamento e o conjunto de teste.
    + Alteração da entrada do construtor para obter uma lista de opções para aplicar as opções imputação para as colunas correspondentes.
    + Correção de erros relacionados à seleção do tipo de latência.
    + Corrigido o erro não classificado gerado nos conjuntos de dados, com refinamentos com a única linha
    + Corrigido o problema com a detecção de frequência lentidão.
    + Corrige um bug no tratamento de exceção do AutoML que causou o verdadeiro motivo para a falha do treinamento ser substituída por um AttributeError.
  + **azureml-CLI-comum**
    + A criação de perfil de instância única foi corrigida para produzir uma recomendação e foi disponibilizada no SDK principal.
  + **azureml-contrib-Mir**
    + Adiciona a funcionalidade na classe MirWebservice para recuperar o token de acesso
    + Usar autenticação de token para MirWebservice por padrão durante MirWebservice. Run () atualizar somente chamada se a chamada falhar
    + A implantação do Mir WebService agora requer SKUs apropriados [Standard_DS2_v2, Standard_F16, Standard_A2_v2] em vez de [Ds2v2, A2v2 e F16] respectivamente.
  + **azureml-contrib-pipeline-etapas**
    + Parâmetro opcional side_inputs adicionado a ParallelRunStep. Esse parâmetro pode ser usado para montar a pasta no contêiner. Atualmente, os tipos com suporte são DataReference e PipelineData.
    + Os parâmetros passados em ParallelRunConfig podem ser substituídos passando parâmetros de pipeline agora. Novos parâmetros de pipeline com suporte aml_mini_batch_size, aml_error_threshold, aml_logging_level aml_run_invocation_timeout (aml_node_count e aml_process_count_per_node já fazem parte da versão anterior).
  + **azureml-núcleo**
    + Os WebServices do AzureML implantados agora `INFO` serão padronizados para registro em log. Isso pode ser controlado pela definição da `AZUREML_LOG_LEVEL` variável de ambiente no serviço implantado.
    + O SDK do Python usa o serviço de descoberta para usar o ponto de extremidade ' API ' em vez de ' pipelines '.
    + Troque para as novas rotas em todas as chamadas do SDK.
    + O roteamento de chamadas para o ModelManagementService foi alterado para uma nova estrutura unificada.
      + Método de atualização do espaço de trabalho disponibilizado publicamente.
      + Parâmetro image_build_compute adicionado no método de atualização do espaço de trabalho para permitir que o usuário atualize a computação para a compilação da imagem.
    + Foram adicionadas mensagens de substituição ao antigo fluxo de trabalho de criação de perfil. Correção dos limites de CPU e memória de criação de perfil.
    + Adicionado RSection como parte do ambiente para executar trabalhos do R.
    + Validação adicionada a `Dataset.mount` para gerar erro quando a origem do conjunto de dados não está acessível ou não contém dados.
    + Adicionado `--grant-workspace-msi-access` como um parâmetro adicional para a CLI do armazenamento de datastore para registrar o contêiner de blob do Azure, que permitirá que você registre o contêiner de BLOB que está atrás de uma VNet.
    + A criação de perfil de instância única foi corrigida para produzir uma recomendação e foi disponibilizada no SDK principal.
    + Correção do problema no aks.py _deploy.
    + Valida a integridade dos modelos que estão sendo carregados para evitar falhas de armazenamento silencioso.
    + Agora, o usuário pode especificar um valor para a chave de autenticação ao regenerar chaves para WebServices.
    + Corrigido o bug em que letras maiúsculas não podem ser usadas como nome de entrada do conjunto de dados.
  + **azureml-padrões**
    + `azureml-dataprep`Agora será instalado como parte do `azureml-defaults`. Não é mais necessário instalar o dataprep [fusível] manualmente em destinos de computação para montar conjuntos de os.
  + **azureml-interpret**
    + Atualizado o azureml-interpretar para interpretar-Community 0,6. *
    + Atualizado o azureml-interpretar para depender do 0.5.0 da comunidade de interpretação
    + Exceções do estilo do azureml adicionadas ao azureml-interpret
    + Correção de serialização de DeepScoringExplainer para modelos de Keras
  + **azureml-mlflow**
    + Adicionar suporte para nuvens soberanas a azureml. mlflow
  + **azureml-pipeline-Core**
    + O notebook de Pontuação do lote de pipeline agora usa ParallelRunStep
    + Correção de um bug em que os resultados de PythonScriptStep podem ser reutilizados incorretamente apesar de alterar a lista de argumentos
    + Adicionada a capacidade de definir o tipo de colunas ao chamar os métodos parse_ * em`PipelineOutputFileDataset`
  + **azureml-pipeline-steps**
    + Moveu `AutoMLStep` o para `azureml-pipeline-steps` o pacote. Preteriu `AutoMLStep` o `azureml-train-automl-runtime`no.
    + Exemplo de documentação adicionada para o conjunto de dados como entrada PythonScriptStep
  + **azureml-tensorboard**
    + atualizado o azureml-tensorboard para dar suporte ao tensorflow 2,0
    + Mostrar o número de porta correto ao usar uma porta Tensorboard personalizada em uma instância de computação
  + **azureml-train-automl-client**
    + Corrigido um problema em que determinados pacotes podem ser instalados em versões incorretas em execuções remotas.
    + correção do problema de substituição de FeaturizationConfig que filtra a configuração personalização personalizada.
  + **azureml-Train-automl-Runtime**
    + Correção do problema com a detecção de frequência nas execuções remotas
    + Moveu `AutoMLStep` o no `azureml-pipeline-steps` pacote. Preteriu `AutoMLStep` o `azureml-train-automl-runtime`no.
  + **azureml-train-core**
    + Dando suporte ao PyTorch versão 1,4 no estimador de PyTorch
  
## <a name="2020-03-02"></a>2020-03-02

### <a name="azure-machine-learning-sdk-for-python-v112rc0-pre-release"></a>SDK do Azure Machine Learning para Python v 1.1.2 RC0 (pré-lançamento)

+ **Correções de bugs e melhorias**
  + **azureml-automl-core**
    + Habilitada a inferência de modo de lote (levando várias linhas uma vez) para modelos automl ONNX
    + Melhoria da detecção de frequência nos conjuntos de dados, falta de dados ou contém pontos de dados irregulares
    + Foi adicionada a capacidade de remover pontos de dados que não estão em conformidade com o frequrncy dominante.
  + **azureml-automl-tempo de execução**
    + Corrigido o problema com o erro gerado se o detalhamento que não estava presente no conjunto de treinamento apareceu no conjunto de testes
    + Removida a y_query requisito durante a pontuação no serviço de previsão
  + **azureml-contrib-Mir**
    + Adiciona a funcionalidade na classe MirWebservice para recuperar o token de acesso
  + **azureml-núcleo**
    + Os WebServices do AzureML implantados agora `INFO` serão padronizados para registro em log. Isso pode ser controlado pela definição da `AZUREML_LOG_LEVEL` variável de ambiente no serviço implantado.
    + Corrija a iteração em `Dataset.get_all` para retornar todos os conjuntos de valores registrados com o espaço de trabalho.
    + Melhorar a mensagem de erro quando o tipo inválido `path` é passado para o argumento de APIs de criação de conjunto de texto.
    + O SDK do Python usa o serviço de descoberta para usar o ponto de extremidade ' API ' em vez de ' pipelines '.
    + Alternar para as novas rotas em todas as chamadas do SDK
    + Altera o roteamento de chamadas para o ModelManagementService para uma nova estrutura unificada
      + Método de atualização do espaço de trabalho disponibilizado publicamente.
      + Parâmetro image_build_compute adicionado no método de atualização do espaço de trabalho para permitir que o usuário atualize a computação para a compilação da imagem
    +  Foram adicionadas mensagens de substituição ao antigo fluxo de trabalho de criação de perfil. Correção de limites de CPU e memória de criação de perfil
  + **azureml-interpret**
    + atualizar o azureml-interpretar para interpretação-comunidade 0,6. *
  + **azureml-mlflow**
    + Adicionar suporte para nuvens soberanas a azureml. mlflow
  + **azureml-pipeline-steps**
    + Movido `AutoMLStep` para o `azureml-pipeline-steps package`. Preteriu `AutoMLStep` o `azureml-train-automl-runtime`no.
  + **azureml-train-automl-client**
    + Corrigido um problema em que determinados pacotes podem ser instalados em versões incorretas em execuções remotas.
  + **azureml-Train-automl-Runtime**
    + Correção do problema com a detecção de frequência nas execuções remotas
    + Movido `AutoMLStep` para o `azureml-pipeline-steps package`. Preteriu `AutoMLStep` o `azureml-train-automl-runtime`no.
  + **azureml-train-core**
    + Movido `AutoMLStep` para o `azureml-pipeline-steps package`. Preteriu `AutoMLStep` o `azureml-train-automl-runtime`no.

## <a name="2020-02-18"></a>2020-02-18

### <a name="azure-machine-learning-sdk-for-python-v111rc0-pre-release"></a>SDK do Azure Machine Learning para Python v 1.1.1 RC0 (pré-lançamento)

+ **Correções de bugs e melhorias**
  + **azure-cli-ml**
    + A criação de perfil de instância única foi corrigida para produzir uma recomendação e foi disponibilizada no SDK principal.
  + **azureml-automl-core**
    + O log de erros foi aprimorado.
  + **azureml-automl-tempo de execução**
    + Corrigido o problema com a previsão quando o conjunto de dados contém granulações curtas com intervalos de tempo longos.
    + Corrigido o problema quando o horizonte máximo automático é ativado e a coluna data contém datas na forma de cadeias de caracteres. Adicionamos uma conversão adequada e um erro sensato se a conversão em data não for possível
    + Usando NumPy e SciPy nativos para serialização e desserialização de dados intermediários para FileCacheStore (usado para execuções locais de AutoML)
    + Corrigido um bug em que as execuções de filho com falha poderiam ficar paralisadas no estado de execução.
  + **azureml-CLI-comum**
    + A criação de perfil de instância única foi corrigida para produzir uma recomendação e foi disponibilizada no SDK principal.
  + **azureml-núcleo**
    + Adicionado `--grant-workspace-msi-access` como um parâmetro adicional para a CLI do armazenamento de datastore para registrar o contêiner de blob do Azure, que permitirá que você registre o contêiner de BLOB que está atrás de uma VNet
    + A criação de perfil de instância única foi corrigida para produzir uma recomendação e foi disponibilizada no SDK principal.
    + Correção do problema no aks.py _deploy
    + Valida a integridade dos modelos que estão sendo carregados para evitar falhas de armazenamento silencioso.
  + **azureml-interpret**
    + exceções do estilo do azureml adicionadas ao azureml-interpret
    + correção de serialização de DeepScoringExplainer para modelos de Keras
  + **azureml-pipeline-Core**
    + O notebook de Pontuação do lote de pipeline agora usa ParallelRunStep
  + **azureml-pipeline-steps**
    + Moveu `AutoMLStep` o no `azureml-pipeline-steps` pacote. Preteriu `AutoMLStep` o `azureml-train-automl-runtime`no.
  + **azureml-contrib-pipeline-etapas**
    + Parâmetro opcional side_inputs adicionado a ParallelRunStep. Esse parâmetro pode ser usado para montar a pasta no contêiner. Atualmente, os tipos com suporte são DataReference e PipelineData.
  + **azureml-tensorboard**
    + atualizado o azureml-tensorboard para dar suporte ao tensorflow 2,0
  + **azureml-train-automl-client**
    + correção do problema de substituição de FeaturizationConfig que filtra a configuração personalização personalizada.
  + **azureml-Train-automl-Runtime**
    + Moveu `AutoMLStep` o no `azureml-pipeline-steps` pacote. Preteriu `AutoMLStep` o `azureml-train-automl-runtime`no.
  + **azureml-train-core**
    + Dando suporte ao PyTorch versão 1,4 no estimador de PyTorch
  
## <a name="2020-02-04"></a>2020-02-04

### <a name="azure-machine-learning-sdk-for-python-v110rc0-pre-release"></a>SDK do Azure Machine Learning para Python v 1.1.0 RC0 (pré-lançamento)

+ **Alterações de quebra**
  + **Controle de Versão Semântico 2.0.0**
    + A partir da versão 1,1, o SDK do Python do Azure ML adota o controle de versão semântico 2.0.0. [Leia mais aqui](https://semver.org/). Todas as versões subsequentes seguirão o novo esquema de numeração e o contrato de controle de versão semântico. 
  
+ **Correções de bugs e melhorias**
  + **azureml-automl-tempo de execução**
    + Maior velocidade de personalização.
    + Correção da verificação de frequência durante a pontuação, agora nas tarefas de previsão, não exigimos equivalência de frequência estrita entre o treinamento e o conjunto de teste.
  + **azureml-núcleo**
    + Agora, o usuário pode especificar um valor para a chave de autenticação ao regenerar chaves para WebServices.
  + **azureml-interpret**
    + Atualizado o azureml-interpretar para depender do 0.5.0 da comunidade de interpretação
  + **azureml-pipeline-Core**
    + Correção de um bug em que os resultados de PythonScriptStep podem ser reutilizados incorretamente apesar de alterar a lista de argumentos
  + **azureml-pipeline-steps**
    + Exemplo de documentação adicionada para o conjunto de dados como entrada PythonScriptStep
  + **azureml-contrib-pipeline-etapas**
    + Os parâmetros passados em ParallelRunConfig podem ser substituídos passando parâmetros de pipeline agora. Novos parâmetros de pipeline com suporte aml_mini_batch_size, aml_error_threshold, aml_logging_level aml_run_invocation_timeout (aml_node_count e aml_process_count_per_node já fazem parte da versão anterior).
  
## <a name="2020-01-21"></a>2020-01-21

### <a name="azure-machine-learning-sdk-for-python-v1085"></a>Azure Machine Learning SDK para Python v 1.0.85

+ **Novos recursos**
  + **azureml-núcleo**
    + Obtenha o uso principal atual e a limitação de cota para recursos de AmlCompute em um determinado espaço de trabalho e assinatura
  
  + **azureml-contrib-pipeline-etapas**
    + Habilitar o usuário a passar o conjunto de resultados de tabela como resultado intermediário da etapa anterior para parallelrunstep

+ **Correções de bugs e melhorias**
  + **azureml-automl-tempo de execução**
    + Removida a necessidade de y_query coluna na solicitação para o serviço de previsão implantado. 
    + O ' y_query ' foi removido da seção de solicitação de serviço de bloco de anotações de suco de Dominick de laranja.
    + Correção do bug que impede a previsão nos modelos implantados, operando em conjuntos de dados com colunas de data e hora.
    + O coeficiente de correlação de Matthews foi adicionado como uma métrica de classificação, para classificação binária e multiclasse.
  + **azureml-contrib-interpret**
    + Os explicadores de texto removidos do azureml-contrib-interprete como explicações de texto foram movidos para o repositório de texto de interpretação que será liberado em breve.
  + **azureml-núcleo**
    + Conjunto de conjuntos: os usos do conjunto de arquivos não dependem mais do numpy e do pandas para serem instalados no Python env.
    + Foi alterado LocalWebservice. wait_for_deployment () para verificar o status do contêiner do Docker local antes de tentar executar ping no ponto de extremidade de integridade, reduzindo muito o tempo necessário para relatar uma implantação com falha.
    + Correção da inicialização de uma propriedade interna usada em LocalWebservice. reload () quando o objeto de serviço é criado a partir de uma implantação existente usando o Construtor LocalWebservice ().
    + Mensagem de erro editada para fins de esclarecimento.
    + Adicionado um novo método chamado get_access_token () a AksWebservice que retornará o objeto AksServiceAccessToken, que contém o token de acesso, atualize após o carimbo de data/hora, expiração em carimbo de data/hora e tipo de token. 
    + O método existente de get_token () preterido em AksWebservice como o novo método retorna todas as informações retornadas por esse método.
    + Saída modificada do comando AZ ml Service Get-Access-token. Token renomeado para accessToken e refreshBy para refreshAfter. Propriedades expiryOn e TokenType adicionadas.
    + Get_active_runs fixo
  + **azureml-explain-model**
    + atualizado o shap para 0.33.0 e interprete-Community para 0,4. *
  + **azureml-interpret**
    + atualizado o shap para 0.33.0 e interprete-Community para 0,4. *
  + **azureml-Train-automl-Runtime**
    + O coeficiente de correlação de Matthews foi adicionado como uma métrica de classificação, para classificação binária e multiclasse.
    + Substituir o sinalizador de pré-processamento do código e substituído por personalização-personalização está ativado por padrão

## <a name="2020-01-06"></a>2020-01-06

### <a name="azure-machine-learning-sdk-for-python-v1083"></a>Azure Machine Learning SDK para Python v 1.0.83

+ **Novos recursos**
  + Conjunto de dados: Adicione `on_error` duas `out_of_range_datetime` opções `to_pandas_dataframe` e para que o falhe quando os dados tiverem valores de erro `None`em vez de preenchê-los com.
  + Espaço de trabalho: `hbi_workspace` adicionou o sinalizador para espaços de trabalho com dados confidenciais que habilitam a criptografia adicional e desabilita o diagnóstico avançado em espaços de trabalho. Também adicionamos suporte para trazer suas próprias chaves para a instância de Cosmos DB associada, especificando os `cmk_keyvault` parâmetros `resource_cmk_uri` e ao criar um espaço de trabalho, que cria uma instância de Cosmos DB em sua assinatura ao provisionar seu espaço de trabalho. [Leia mais aqui.](https://docs.microsoft.com/azure/machine-learning/concept-enterprise-security#azure-cosmos-db)

+ **Correções de bugs e melhorias**
  + **azureml-automl-tempo de execução**
    + Correção de uma regressão que causou a geração de um TypeError ao executar AutoML em versões do Python abaixo de 3.5.4.
  + **azureml-núcleo**
    + Corrigido o bug `datastore.upload_files` em que o caminho relativo que não `./` iniciou com não pôde ser usado.
    + Mensagens de substituição adicionadas para todas as classes de imagem caminhos
    + A construção de URL de Gerenciamento de Modelos fixa para a região mooncake.
    + Correção do problema em que os modelos que usam source_dir não puderam ser empacotados para Azure Functions.    
    + Adicionada uma opção ao [Environment. build_local ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) para enviar uma imagem para o registro de contêiner do espaço de trabalho do AzureML
    + O SDK foi atualizado para usar a nova biblioteca de tokens no Azure Synapse de forma compatível.
  + **azureml-interpret**
    + Corrigido o bug onde nenhum foi retornado quando nenhuma explicação estava disponível para download. Agora gera uma exceção, correspondendo ao comportamento em outro lugar.
  + **azureml-pipeline-steps**
    + Não permitido `DatasetConsumptionConfig`passar de s para `Estimator`o `inputs` parâmetro do quando `Estimator` o será usado em um `EstimatorStep`.
  + **azureml-SDK**
    + Adicionado o cliente AutoML ao pacote azureml-SDK, permitindo que as execuções de AutoML remotas sejam enviadas sem instalar o pacote completo de AutoML.
  + **azureml-train-automl-client**
    + Alinhamento corrigido na saída do console para execuções de automl
    + Corrigido um bug em que a versão incorreta do pandas pode ser instalada em amlcompute remotas.

## <a name="2019-12-23"></a>2019-12-23

### <a name="azure-machine-learning-sdk-for-python-v1081"></a>Azure Machine Learning SDK para Python v 1.0.81

+ **Correções de bugs e melhorias**
  + **azureml-contrib-interpret**
    + adiar dependência de shap para interpretação-Community do azureml-interpret
  + **azureml-núcleo**
    + O destino de computação agora pode ser especificado como um parâmetro para os objetos de configuração de implantação correspondentes. Isso é especificamente o nome do destino de computação no qual implantar, não o objeto do SDK.
    + Adicionadas informações de CreatedBy a objetos de modelo e de serviço. Pode ser acessado por meio <var>de. created_by
    + Foi corrigido ContainerImage. Run (), que não configurava corretamente a porta HTTP do contêiner do Docker.
    + Tornar `azureml-dataprep` opcional para `az ml dataset register` comando da CLI
    + Correção de um bug `TabularDataset.to_pandas_dataframe` em que o retornoia incorreto para um leitor alternativo e imprime um aviso.
  + **azureml-explain-model**
    + adiar dependência de shap para interpretação-Community do azureml-interpret
  + **azureml-pipeline-Core**
    + Adicionada nova etapa `NotebookRunnerStep`de pipeline para executar um bloco de anotações local como uma etapa no pipeline.
    + Funções de get_all preteridas removidas para PublishedPipelines, agendas e PipelineEndpoints
  + **azureml-train-automl-client**
    + Foi iniciada a reprovação de data_script como uma entrada para AutoML.


## <a name="2019-12-09"></a>09/12/2019

### <a name="azure-machine-learning-sdk-for-python-v1079"></a>Azure Machine Learning SDK para Python v 1.0.79

+ **Correções de bugs e melhorias**
  + **azureml-automl-core**
    + FeaturizationConfig removidas para registro em log
      + Atualizado o log para o log "auto"/"off"/"personalizado" apenas.
  + **azureml-automl-tempo de execução**
    + Adicionado suporte para pandas. Série e pandas. Categórico para detectar o tipo de dados da coluna. Anteriormente, só há suporte para numpy. ndarray
      + Foram adicionadas alterações de código relacionadas para lidar com dtype categóricos corretamente.
    + A interface da função de previsão foi aprimorada: o parâmetro y_pred foi tornado opcional. -Os docstrings foram aprimorados.
  + **azureml-contrib-dataset**
    + Correção de um bug em que não foi possível montar os conjuntos de itens rotulados.
  + **azureml-núcleo**
    + Correção de bug `Environment.from_existing_conda_environment(name, conda_environment_name)`para. O usuário pode criar uma instância do ambiente que é a réplica exata do ambiente local
    + Alterados métodos de conjuntos de valores relacionados à série `include_boundary=True` temporal para por padrão.
  + **azureml-train-automl-client**
    + Corrigido o problema em que os resultados da validação não são impressos quando mostrar saída está definido como false.


## <a name="2019-11-25"></a>2019-11-25

### <a name="azure-machine-learning-sdk-for-python-v1076"></a>Azure Machine Learning SDK para Python v 1.0.76

+ **Alterações de quebra**
  + Problemas de atualização do Azureml-Train-AutoML
    + Atualizando para o azureml-Train-automl>= 1.0.76 do azureml-Train-automl<1.0.76 pode causar instalações parciais, fazendo com que algumas importações de automl falhem. Para resolver isso, você pode executar o script de instalação encontrado https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/automl_setup.cmdem. Ou, se você estiver usando o Pip diretamente, poderá:
      + "instalação de Pip--atualizar o azureml-Train-automl"
      + "instalação de Pip--ignorar-instalado azureml-Train-automl-Client"
    + ou você pode desinstalar a versão antiga antes de atualizar
      + "desinstalação de Pip azureml-Train-automl"
      + "instalação de Pip azureml-Train-automl"

+ **Correções de bugs e melhorias**
  + **azureml-automl-tempo de execução**
    + AutoML agora levará em conta as classes true e false ao calcular as métricas escalares médias para tarefas de classificação binária.
    + O aprendizado de máquina e o código de treinamento foram movidos no AzureML-AutoML-Core para um novo pacote AzureML-AutoML-Runtime.
  + **azureml-contrib-dataset**
    + Ao chamar `to_pandas_dataframe` em um DataSet rotulado com a opção de download, agora você pode especificar se deseja substituir os arquivos existentes ou não.
    + Ao chamar `keep_columns` ou `drop_columns` que resulta em uma coluna de timeseries, rótulo ou imagem sendo removida, os recursos correspondentes também serão descartados para o conjunto de resultados.
    + Corrigido um problema com o carregador de pytorch para a tarefa de detecção de objeto.
  + **azureml-contrib-interpret**
    + O widget de explicação Dashboard foi removido de azureml-contrib-interpret, pacote alterado para referenciar o novo no interpret_community
    + Versão atualizada da interpretação-Community para 0.2.0
  + **azureml-núcleo**
    + Melhorar o desempenho `workspace.datasets`do.
    + Foi adicionada a capacidade de registrar o armazenamento de dados do Azure SQL usando o nome de usuário e a autenticação de senha
    + Correção para carregar RunConfigurations de caminhos relativos.
    + Ao chamar `keep_columns` ou `drop_columns` que resulta em uma coluna da série temporal sendo removida, os recursos correspondentes também serão descartados para o conjunto de resultados.
  + **azureml-interpret**
    + versão atualizada da interpretação-Community para 0.2.0
  + **azureml-pipeline-steps**
    + Valores com suporte documentados para `runconfig_pipeline_params` as etapas de pipeline do Azure Machine Learning.
  + **azureml-pipeline-Core**
    + Adicionada a opção de CLI para baixar a saída no formato JSON para comandos de pipeline.
  + **azureml-Train-automl**
    + Divida o AzureML-Train-AutoML em 2 pacotes, um pacote do cliente AzureML-Train-AutoML-Client e um pacote de treinamento de ML AzureML-Train-AutoML-Runtime
  + **azureml-train-automl-client**
    + Adicionou um cliente fino para enviar experimentos AutoML sem a necessidade de instalar nenhuma dependência de Machine Learning localmente.
    + Registro de log fixo de atraso detectado, tamanhos de janela sem interrupção e horizontes máximos em execuções remotas detectadas automaticamente.
  + **azureml-Train-automl-Runtime**
    + Foi adicionado um novo pacote AutoML para isolar os componentes de aprendizado de máquina e tempo de execução do cliente.
  + **azureml-contrib-Train-RL**
    + Adicionado suporte do reforço Learning no SDK.
    + Adicionado suporte a AmlWindowsCompute no RL SDK.


## <a name="2019-11-11"></a>2019-11-11

### <a name="azure-machine-learning-sdk-for-python-v1074"></a>Azure Machine Learning SDK para Python v 1.0.74

  + **Recursos de visualização**
    + **azureml-contrib-dataset**
      + Depois de importar o azureml-contrib-DataSet, você `Dataset.Labeled.from_json_lines` pode chamar `._Labeled` em vez de criar um conjunto de um DataSet.
      + Ao chamar `to_pandas_dataframe` em um DataSet rotulado com a opção de download, agora você pode especificar se deseja substituir os arquivos existentes ou não.
      + Ao chamar `keep_columns` ou `drop_columns` que resulta em uma série temporal, rótulo ou coluna de imagem sendo descartada, os recursos correspondentes também serão descartados para o conjunto de resultados.
      + Correção de problemas com o carregador PyTorch `dataset.to_torchvision()`ao chamar.

+ **Correções de bugs e melhorias**
  + **azure-cli-ml**
    + Adicionada a criação de perfil de modelo à CLI de visualização.
    + Corrige alterações significativas no armazenamento do Azure fazendo com que a CLI do AzureML falhe.
    + Adicionado Load Balancer tipo a MLC para tipos AKS
  + **azureml-automl-core**
    + Corrigido o problema com a detecção de horizonte máximo na série temporal, tendo valores ausentes e vários refinamentos.
    + Corrigido o problema com falhas durante a geração de divisões de validação cruzada.
    + Substitua esta seção por uma mensagem no formato de redução para aparecer nas notas de versão: tratamento aprimorado de refinamentos em conjuntos de dados de previsão.
    + Correção do problema com o mascaramento de algumas informações do usuário durante o registro em log. -Registro em log dos erros aprimorados durante a previsão de execuções.
    + Adicionar psutil como uma dependência Conda ao arquivo de implantação yml gerado automaticamente.
  + **azureml-contrib-Mir**
    + Corrige alterações significativas no armazenamento do Azure fazendo com que a CLI do AzureML falhe.
  + **azureml-núcleo**
    + Corrige um bug que causou a implantação de modelos em Azure Functions para produzir o 500S.
    + Corrigido um problema em que o arquivo amlignore não foi aplicado em instantâneos.
    + Adicionada uma nova API amlcompute. get_active_runs que retorna um gerador para execução e em fila é executado em um determinado amlcompute.
    + Adicionado Load Balancer tipo a MLC para tipos AKS.
    + Adicionado append_prefix parâmetro BOOL para download_files em run.py e download_artifacts_from_prefix no artifacts_client. Esse sinalizador é usado para mesclar seletivamente o FilePath de origem para que apenas o nome de arquivo ou pasta seja adicionado ao output_directory
    + Corrigir o problema de desserialização para com o uso do conjunto de `run_config.yml` um.
    + Ao chamar `keep_columns` ou `drop_columns` que resulta em uma coluna de série temporal sendo removida, os recursos correspondentes também serão descartados para o conjunto de resultados.
  + **azureml-interpret**
    + Atualizada a versão de interpretação da Comunidade para 0.1.0.3
  + **azureml-Train-automl**
    + Corrigido um problema em que automl_step pode não imprimir problemas de validação.
    + O register_model corrigido para ter sucesso mesmo que o ambiente do modelo não tenha dependências locais.
    + Corrigido um problema em que algumas execuções remotas não estavam habilitadas para o Docker.
    + Adicione o log da exceção que está causando a falha prematura de uma execução local.
  + **azureml-train-core**
    + Considere resume_from execuções no cálculo de melhores execuções de filhos de ajuste de hiperparâmetro.
  + **azureml-pipeline-Core**
    + Manipulação de parâmetro fixo na construção de argumento de pipeline.
    + Adicionada a descrição de pipeline e o parâmetro de tipo de etapa YAML.
    + Novo formato YAML para etapa de pipeline e aviso de substituição adicionado para o formato antigo.



## <a name="2019-11-04"></a>2019-11-04

### <a name="web-experience"></a>Experiência na Web

A página de aterrissagem do espaço [https://ml.azure.com](https://ml.azure.com) de trabalho colaborativo em foi aprimorada e remarcada como o Azure Machine Learning Studio (versão prévia).

No estúdio, você pode treinar, testar, implantar e gerenciar Azure Machine Learning ativos, como conjuntos de informações, pipelines, modelos, pontos de extremidade e muito mais.

Acesse as seguintes ferramentas de criação baseadas na Web no estúdio:

| Ferramenta baseada na Web | Descrição | Edition |
|-|-|-|
| VM do notebook (visualização) | Estação de trabalho baseada em nuvem totalmente gerenciada | Básico & Enterprise |
| [Machine Learning automatizado](tutorial-first-experiment-automated-ml.md) (visualização) | Não há experiência de código para automatizar o desenvolvimento de modelos do Machine Learning | Enterprise |
| [Designer](concept-designer.md) (versão prévia) | Ferramenta de modelagem de aprendizado de máquina de arrastar e soltar anteriormente conhecida como o designer | Enterprise |


### <a name="azure-machine-learning-designer-enhancements"></a>Aprimoramentos do designer de Azure Machine Learning

+ Anteriormente conhecido como interface visual 
+    11 novos [módulos](algorithm-module-reference/module-reference.md) , incluindo recomendados, classificadores e utilitários de treinamento, incluindo engenharia de recursos, validação cruzada e transformação de dados.

### <a name="r-sdk"></a>SDK do R 
 
Os cientistas de dados e os desenvolvedores de ia usam o [SDK do Azure Machine Learning para R](tutorial-1st-r-experiment.md) para criar e executar fluxos de trabalho de aprendizado de máquina com Azure Machine Learning.

O SDK do Azure Machine Learning para R usa `reticulate` o pacote para associar ao SDK do Python. Ligando diretamente ao Python, o SDK para R permite que você acesse os principais objetos e métodos implementados no SDK do Python de qualquer ambiente de R que você escolher.

Os principais recursos do SDK incluem:

+    Gerenciar recursos de nuvem para monitorar, registrar em log e organizar seus experimentos de machine learning.
+    Treine modelos usando recursos de nuvem, incluindo treinamento de modelo com aceleração de GPU.
+    Implante seus modelos como WebServices em ACI (instâncias de contêiner do Azure) e AKS (serviço kubernetes do Azure).

Consulte o [site do pacote](https://azure.github.io/azureml-sdk-for-r) para obter a documentação completa.

### <a name="azure-machine-learning-integration-with-event-grid"></a>Integração do Azure Machine Learning com a grade de eventos 

Azure Machine Learning agora é um provedor de recursos para a grade de eventos, você pode configurar eventos de Machine Learning por meio do portal do Azure ou CLI do Azure. Os usuários podem criar eventos para conclusão de execução, registro de modelo, implantação de modelo e descompasso de dados detectados. Esses eventos podem ser roteados para manipuladores de eventos com suporte na grade de eventos para consumo. Consulte o [esquema](https://docs.microsoft.com/azure/event-grid/event-schema-machine-learning)de eventos do Machine Learning, os [conceitos](https://docs.microsoft.com/azure/machine-learning/concept-event-grid-integration) e artigos do [tutorial](https://docs.microsoft.com/azure/machine-learning/how-to-use-event-grid) para obter mais detalhes.

## <a name="2019-10-31"></a>2019-10-31

### <a name="azure-machine-learning-sdk-for-python-v1072"></a>Azure Machine Learning SDK para Python v 1.0.72

+ **Novos recursos**
  + Os monitores de conjunto de dados adicionados por meio do pacote [**azureml-descompasso**](https://docs.microsoft.com/python/api/azureml-datadrift) , permitindo o monitoramento de conjuntos de dados de série temporal para descompasso ou outras alterações estatísticas ao longo do tempo. Alertas e eventos podem ser disparados se o descompasso for detectado ou se outras condições nos dados forem atendidas. Consulte [nossa documentação](https://aka.ms/datadrift) para obter detalhes.
  + Anunciando duas novas edições (também chamadas de SKU intercambiáveis) em Azure Machine Learning. Com esta versão, agora você pode criar um espaço de trabalho básico ou de Azure Machine Learning empresarial. Todos os espaços de trabalho existentes serão padronizados para a edição básica e você poderá ir para a portal do Azure ou para o estúdio para atualizar o espaço de trabalho a qualquer momento. Você pode criar um espaço de trabalho básico ou empresarial do portal do Azure. Leia [nossa documentação](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace) para saber mais. No SDK, a edição do seu espaço de trabalho pode ser determinada usando a propriedade "SKU" do seu objeto de espaço de trabalho.
  + Também fizemos aprimoramentos para Azure Machine Learning computação. agora você pode exibir métricas para seus clusters (como nós totais, nós em execução, cota de núcleo total) em Azure Monitor, além de exibir os logs de diagnóstico para depuração. Além disso, você também pode exibir execuções em fila ou atualmente em execução no cluster e detalhes como os IPs dos vários nós no cluster. Você pode exibi-los no portal ou usando funções correspondentes no SDK ou na CLI.

  + **Recursos de visualização**
    + Estamos liberando o suporte de visualização para a criptografia de disco do SSD local no Azure Machine Learning computação. Gere um tíquete de suporte técnico para que sua assinatura seja exibida na lista de permissões para usar esse recurso.
    + Visualização pública de inferência de Azure Machine Learning lote. Azure Machine Learning inferência de lote tem como alvo grandes trabalhos de inferência que não diferenciam o tempo. A inferência de lote fornece o dimensionamento de computação de inferência econômica, com taxa de transferência incomparável para aplicativos assíncronos. Ele é otimizado para inferência de alta taxa de transferência, incêndio e esqueci sobre grandes coleções de dados.
    + [**azureml-contrib-dataset**](https://docs.microsoft.com/python/api/azureml-contrib-dataset)
        + Funcionalidades habilitadas para DataSet rotulado
        ```Python
        import azureml.core
        from azureml.core import Workspace, Datastore, Dataset
        import azureml.contrib.dataset
        from azureml.contrib.dataset import FileHandlingOption, LabeledDatasetTask

        # create a labeled dataset by passing in your JSON lines file
        dataset = Dataset._Labeled.from_json_lines(datastore.path('path/to/file.jsonl'), LabeledDatasetTask.IMAGE_CLASSIFICATION)

        # download or mount the files in the `image_url` column
        dataset.download()
        dataset.mount()

        # get a pandas dataframe
        from azureml.data.dataset_type_definitions import FileHandlingOption
        dataset.to_pandas_dataframe(FileHandlingOption.DOWNLOAD)
        dataset.to_pandas_dataframe(FileHandlingOption.MOUNT)

        # get a Torchvision dataset
        dataset.to_torchvision()
        ```

+ **Correções de bugs e melhorias**
  + **azure-cli-ml**
    + A CLI agora dá suporte ao empacotamento de modelo.
    + Adicionada a CLI de DataSet. Para obter mais informações:`az ml dataset --help`
    + Adicionado suporte para implantação e empacotamento de modelos com suporte (ONNX, scikit-Learn e TensorFlow) sem uma instância InferenceConfig.
    + Foi adicionado o sinalizador de substituição para a implantação do serviço (ACI e AKS) no SDK e na CLI. Se fornecido, substituirá o serviço existente se o serviço com o nome já existir. Se o serviço não existir, o criará um novo serviço.
    + Os modelos podem ser registrados com duas novas estruturas, Onnx e Tensorflow. -O registro de modelo aceita dados de entrada de exemplo, dados de saída de exemplo e configuração de recurso para o modelo.
  + **azureml-automl-core**
    + O treinamento de uma iteração seria executado em um processo filho somente quando as restrições de tempo de execução estiverem sendo definidas.
    + Adição de um Guardrail para tarefas de previsão, para verificar se um max_horizon especificado causará um problema de memória no computador determinado ou não. Se for, uma mensagem Guardrail será exibida.
    + Suporte adicionado para frequências complexas, como 2 anos e 1 mês. -Abrangente mensagem de erro adicionada se a frequência não puder ser determinada.
    + Adicionar azureml-padrões para o Conda env gerado automaticamente para resolver a falha de implantação do modelo
    + Permitir que dados intermediários no pipeline de Azure Machine Learning sejam convertidos em DataSet tabular e usados no `AutoMLStep`.
    + Implementada atualização de finalidade de coluna para streaming.
    + Implementada atualização de parâmetro do transformador para imputer e HashOneHotEncoder para streaming.
    + Adicionou o tamanho de dados atual e o tamanho mínimo de dados exigido para as mensagens de erro de validação.
    + Atualizado o tamanho mínimo de dados necessários para validação cruzada para garantir um mínimo de dois exemplos em cada dobra de validação.
  + **azureml-CLI-comum**
    + A CLI agora dá suporte ao empacotamento de modelo.
    + Os modelos podem ser registrados com duas novas estruturas, Onnx e Tensorflow.
    + O registro de modelo aceita dados de entrada de exemplo, dados de saída de exemplo e configuração de recurso para o modelo.
  + **azureml-contrib-GBDT**
    + Corrigido o canal de liberação para o bloco de anotações
    + Foi adicionado um aviso para o destino de computação não AmlCompute para o qual não há suporte
    + Foi adicionado um estimador de LightGMB para o pacote azureml-contrib-GBDT
  + [**azureml-núcleo**](https://docs.microsoft.com/python/api/azureml-core)
    + A CLI agora dá suporte ao empacotamento de modelo.
    + Adicionar aviso de reprovação para APIs de conjunto de Consulte o aviso de alteração da https://aka.ms/tabular-datasetAPI do conjunto de notícias em.
    + Altere [`Dataset.get_by_id`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29#get-by-id-workspace--id-) para retornar o nome e a versão do registro se o conjunto de registros estiver registrado.
    + Corrija um bug de que ScriptRunConfig com DataSet as argument não pode ser usado repetidamente para enviar a execução do experimento.
    + Os conjuntos de valores recuperados durante uma execução serão rastreados e poderão ser vistos na página de detalhes da [`run.get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--) execução ou chamando após a conclusão da execução.
    + Permitir que dados intermediários no pipeline de Azure Machine Learning sejam convertidos em DataSet tabular e usados no [`AutoMLStep`](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlstep).
    + Adicionado suporte para implantação e empacotamento de modelos com suporte (ONNX, scikit-Learn e TensorFlow) sem uma instância InferenceConfig.
    + Foi adicionado o sinalizador de substituição para a implantação do serviço (ACI e AKS) no SDK e na CLI. Se fornecido, substituirá o serviço existente se o serviço com o nome já existir. Se o serviço não existir, o criará um novo serviço.
    +  Os modelos podem ser registrados com duas novas estruturas, Onnx e Tensorflow. O registro de modelo aceita dados de entrada de exemplo, dados de saída de exemplo e configuração de recurso para o modelo.
    + Adição de novo armazenamento de dados para o Azure Database para MySQL. Exemplo adicionado para usar o banco de dados do Azure para MySQL no DataTransferStep em pipelines de Azure Machine Learning.
    + Funcionalidade adicionada para adicionar e remover marcas de experimentos com funcionalidade adicionada para remover marcas de execuções
    + Foi adicionado o sinalizador de substituição para a implantação do serviço (ACI e AKS) no SDK e na CLI. Se fornecido, substituirá o serviço existente se o serviço com o nome já existir. Se o serviço não existir, o criará um novo serviço.
  + [**azureml-descompasso**](https://docs.microsoft.com/python/api/azureml-datadrift)
    + Movido de `azureml-contrib-datadrift` para`azureml-datadrift`
    + Adicionado suporte para monitoramento de conjuntos de data de série temporal para descompasso e outras medidas estatísticas
    + Novos métodos `create_from_model()` e `create_from_dataset()` para a [`DataDriftDetector`](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector(class)) classe. O `create()` método será preterido.
    + Ajustes nas visualizações no Python e na interface do usuário no Azure Machine Learning Studio.
    + Suporte a agendamento de monitor semanal e mensal, além de diários para monitores de conjuntos de conjunto de DataSet.
    + Suporte a aterramento de métricas de monitor de dados para analisar dados históricos de monitores de conjunto de dados.
    + Correções de vários bugs
  + [**azureml-pipeline-Core**](https://docs.microsoft.com/python/api/azureml-pipeline-core)
    + o azureml-dataprep não é mais necessário para enviar uma execução de pipeline de Azure Machine Learning `yaml` do arquivo de pipeline.
  + [**azureml-Train-automl**](/python/api/azureml-train-automl-runtime/)
    + Adicionar azureml-padrões para o Conda env gerado automaticamente para resolver a falha de implantação do modelo
    + O treinamento remoto do AutoML agora inclui os padrões do azureml para permitir a reutilização de Training env para inferência.
  + **azureml-train-core**
    + Adicionado suporte a PyTorch 1,3 [`PyTorch`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch) no estimador

## <a name="2019-10-21"></a>2019-10-21

### <a name="visual-interface-preview"></a>Interface visual (visualização)

+ A interface visual do Azure Machine Learning (versão prévia) foi revisada para ser executada em [pipelines Azure Machine Learning](concept-ml-pipelines.md). Os pipelines (anteriormente conhecidos como experimentos) criados na interface visual agora estão totalmente integrados com a experiência de Azure Machine Learning principal.
  + Experiência de gerenciamento unificada com ativos do SDK
  + Controle de versão e acompanhamento de modelos de interface visual, pipelines e pontos de extremidade
  + Interface do usuário reprojetada
  + Implantação de inferência de lote adicionada
  + Suporte ao Azure kubernetes Service (AKS) adicionado para os destinos de computação de inferência
  + Fluxo de trabalho de criação do pipeline de etapa do novo Python
  + Nova [página de aterrissagem](https://ml.azure.com) para ferramentas de criação Visual

+ **Novos módulos**
  + Aplicar operação matemática
  + Aplicar transformação SQL
  + Valores de clipe
  + Resumir dados
  + Importar do banco de dados SQL

## <a name="2019-10-14"></a>2019-10-14

### <a name="azure-machine-learning-sdk-for-python-v1069"></a>Azure Machine Learning SDK para Python v 1.0.69

+ **Correções de bugs e melhorias**
  + **azureml-automl-core**
    + Limitação de explicações de modelo para a melhor execução em vez de calcular explicações para cada execução. Fazendo esse comportamento mudar para local, remoto e ADB.
    + Suporte adicionado para explicações de modelo sob demanda para a interface do usuário
    + Adicionou psutil como uma dependência de `automl` e incluiu psutil como uma dependência Conda no amlcompute.
    + Corrigido o problema com tamanhos de janela lentos e contínuos nos conjuntos de dados de previsão, algumas séries que podem causar erros de algebra linear
      + Impressão adicional adicionada para os parâmetros que são determinados de heurística nas execuções de previsão.
  + **azureml-contrib-descompasso**
    + Proteção adicionada ao criar métricas de saída se o descompasso de nível de conjunto de resultados não estiver na primeira seção.
  + **azureml-contrib-interpret**
    + o pacote azureml-contrib-explique-Model foi renomeado para azureml-contrib-interpret
  + **azureml-núcleo**
    + API adicionada para cancelar o registro de conjuntos de os. `dataset.unregister_all_versions()`
    + o pacote azureml-contrib-explique-Model foi renomeado para azureml-contrib-interpret.
  + **[azureml-núcleo](https://docs.microsoft.com/python/api/azureml-core)**
    + API adicionada para cancelar o registro de conjuntos de os. DataSet. [unregister_all_versions ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.abstract_datastore.abstractdatastore#unregister--).
    + Adicionada a API do conjunto de dados para verificar a hora da alteração do dado. `dataset.data_changed_time`.
    + `FileDataset` Poder consumir `TabularDataset` e como entradas para `PythonScriptStep`, `EstimatorStep`e `HyperDriveStep` no pipeline Azure Machine Learning
    + O desempenho `FileDataset.mount` do foi aprimorado para pastas com um grande número de arquivos
    + Ser capaz de consumir [Filedataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset) e [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset) como entradas para [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep), [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimatorstep)e [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyperdrivestep) no pipeline de Azure Machine Learning.
    + Desempenho do filedataset. a [montagem ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset#mount-mount-point-none----kwargs-) foi aprimorada para pastas com um grande número de arquivos
    + URL adicionada a recomendações de erro conhecidas em detalhes da execução.
    + Correção de um bug em execução. get_metrics em que as solicitações falhariam se uma execução tivesse um número excessivo de filhos
    + Correção de um bug em [execução. get_metrics](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run#get-metrics-name-none--recursive-false--run-type-none--populate-false-) em que as solicitações falhariam se uma execução tivesse um número excessivo de filhos
    + Suporte adicionado para autenticação no cluster Arcadia.
    + Criar um objeto de teste Obtém ou cria o experimento no espaço de trabalho Azure Machine Learning para acompanhamento do histórico de execução. A ID do experimento e o tempo arquivado são populados no objeto experimento na criação. Exemplo: experimento = experimento (espaço de trabalho, "novo experimento") experiment_id = experiment.id arquivo morto () e reativar () são funções que podem ser chamadas em um experimento para ocultar e restaurar o experimento de ser mostrado na UX ou retornado por padrão em uma chamada para experimentos de lista. Se um novo experimento for criado com o mesmo nome de um experimento arquivado, você poderá renomear o teste arquivado ao reativar o passando um novo nome. Só pode haver um experimento ativo com um determinado nome. Exemplo: experiment1 = experimento (espaço de trabalho, "teste ativo") experiment1. Archive () # criar novo experimento ativo com o mesmo nome que o arquivado. experiment2. = Experimento (espaço de trabalho, "teste ativo") experiment1. reativar (new_name = "experimento ativo anterior") a lista de métodos estáticos () no experimento pode usar um filtro de nome e um filtro ViewType. Os valores ViewType são "ACTIVE_ONLY", "ARCHIVED_ONLY" e "ALL" exemplo: archived_experiments = experimento. List (Workspace, view_type = "ARCHIVED_ONLY") all_first_experiments = experimento. List (espaço de trabalho, Name = "First experimento", view_type = "ALL")
    + Suporte ao uso do ambiente para implantação de modelo e atualização de serviço
  + **azureml-descompasso**
    + O atributo show da classe DataDriftDector não dará suporte a um argumento opcional ' with_details '. O atributo show só apresentará o coeficiente de descompasso de dados e a contribuição de descompasso de dados das colunas de recursos.
    + Alterações de comportamento do atributo DataDriftDetector ' get_output ':
      + O parâmetro de entrada start_time, end_time são opcionais, em vez de obrigatórios;
      + As start_time específicas de entrada e/ou end_time com uma run_id específica na mesma chamada resultarão na exceção de erro de valor porque são mutuamente exclusivas
      + Por entrada específica start_time e/ou end_time, somente os resultados das execuções agendadas serão retornados;
      + O parâmetro ' daily_latest_only ' foi preterido.
    + Suporte à recuperação de saídas de descompasso de dados baseados em DataSet.
  + **azureml-explain-model**
    + Renomeia o pacote AzureML-explique-Model para o AzureML-interpret, mantendo o pacote antigo para compatibilidade com versões anteriores agora
    + correção `automl` de bug com explicações brutas definido como tarefa de classificação em vez de regressão por padrão no download de ExplanationClient
    + Adicionar suporte para `ScoringExplainer` que seja criado diretamente usando`MimicWrapper`
  + **azureml-pipeline-Core**
    + Desempenho aprimorado para criação de pipeline grande
  + **azureml-train-core**
    + Adicionado suporte TensorFlow 2,0 no estimador TensorFlow
  + **azureml-Train-automl**
    + Criar um objeto de [teste](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment) Obtém ou cria o experimento no espaço de trabalho Azure Machine Learning para acompanhamento do histórico de execução. A ID do experimento e o tempo arquivado são populados no objeto experimento na criação. Exemplo:

        ```py
        experiment = Experiment(workspace, "New Experiment")
        experiment_id = experiment.id
        ```
        [arquivo morto ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#archive--) e [reativar ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#reactivate-new-name-none-) são funções que podem ser chamadas em um experimento para ocultar e restaurar o experimento de ser mostrado na UX ou retornado por padrão em uma chamada para experimentos de lista. Se um novo experimento for criado com o mesmo nome de um experimento arquivado, você poderá renomear o teste arquivado ao reativar o passando um novo nome. Só pode haver um experimento ativo com um determinado nome. Exemplo:

        ```py
        experiment1 = Experiment(workspace, "Active Experiment")
        experiment1.archive()
        # Create new active experiment with the same name as the archived.
        experiment2 = Experiment(workspace, "Active Experiment")
        experiment1.reactivate(new_name="Previous Active Experiment")
        ```
        A lista de métodos estáticos [()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#list-workspace--experiment-name-none--view-type--activeonly---tags-none-) no experimento pode usar um filtro de nome e um filtro ViewType. Os valores ViewType são "ACTIVE_ONLY", "ARCHIVED_ONLY" e "ALL". Exemplo:

        ```py
        archived_experiments = Experiment.list(workspace, view_type="ARCHIVED_ONLY")
        all_first_experiments = Experiment.list(workspace, name="First Experiment", view_type="ALL")
        ```
    + Suporte ao uso do ambiente para implantação de modelo e atualização de serviço.
  + **[azureml-descompasso](https://docs.microsoft.com/python/api/azureml-datadrift)**
    + O atributo show da classe [DataDriftDetector](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector) não dará suporte a um argumento opcional ' with_details '. O atributo show só apresentará o coeficiente de descompasso de dados e a contribuição de descompasso de dados das colunas de recursos.
    + Alterações de comportamento de funçãohttps://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector#get-output-start-time-none--end-time-none--run-id-none-) DataDriftDetector [get_output]:
      + O parâmetro de entrada start_time, end_time são opcionais, em vez de obrigatórios;
      + As start_time específicas de entrada e/ou end_time com uma run_id específica na mesma invocação resultarão na exceção de erro de valor, pois são mutuamente exclusivas;
      + Por entrada específica start_time e/ou end_time, somente os resultados das execuções agendadas serão retornados;
      + O parâmetro ' daily_latest_only ' foi preterido.
    + Suporte à recuperação de saídas de descompasso de dados baseados em DataSet.
  + **azureml-explain-model**
    + Renomeia o pacote AzureML-explique-Model para o AzureML-interpret, mantendo o pacote antigo para compatibilidade com versões anteriores por enquanto.
    + correção do bug AutoML com explicações brutas definidas para tarefa de classificação em vez de regressão por padrão no download de ExplanationClient.
    + Adicionar suporte para [ScoringExplainer](/python/api/azureml-interpret/azureml.interpret.scoring.scoring_explainer.scoringexplainer?view=azure-ml-py) a ser criado diretamente usando MimicWrapper
  + **[azureml-pipeline-Core](https://docs.microsoft.com/python/api/azureml-pipeline-core)**
    + Desempenho aprimorado para criação de pipeline grande.
  + **[azureml-train-core](https://docs.microsoft.com/python/api/azureml-train-core)**
    + Adicionado suporte do TensorFlow 2,0 no estimador do [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow) .
  + **[azureml-Train-automl](/python/api/azureml-train-automl-runtime/)**
    + A execução do pai não será mais possível quando a iteração da instalação falhar, pois a orquestração já cuida dela.
    + Adicionado suporte local-Docker e Conda local para experimentos de AutoML
    + Adicionado suporte local-Docker e Conda local para experimentos de AutoML.


## <a name="2019-10-08"></a>08-10-2019

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Nova experiência na Web (versão prévia) para espaços de trabalho Azure Machine Learning

A guia experimento no [novo portal de espaço de trabalho](https://ml.azure.com) foi atualizada para que os cientistas de dados possam monitorar experimentos de forma mais eficaz. Você pode explorar os seguintes recursos:
+ Teste os metadados para filtrar e classificar facilmente sua lista de experimentos
+ Páginas de detalhes do experimento simplificado e de alto desempenho que permitem visualizar e comparar suas execuções
+ Novo design para executar páginas de detalhes para entender e monitorar as execuções de treinamento

## <a name="2019-09-30"></a>2019-09-30

### <a name="azure-machine-learning-sdk-for-python-v1065"></a>Azure Machine Learning SDK para Python v 1.0.65

  + **Novos recursos**
    + Adicionados ambientes organizados. Esses ambientes foram pré-configurados com bibliotecas para tarefas comuns de aprendizado de máquina e foram previamente compilados e armazenados em cache como imagens do Docker para uma execução mais rápida. Eles aparecem por padrão na lista de ambiente do espaço de trabalho, com o prefixo "AzureML".
    + Adicionados ambientes organizados. Esses ambientes foram pré-configurados com bibliotecas para tarefas comuns de aprendizado de máquina e foram previamente compilados e armazenados em cache como imagens do Docker para uma execução mais rápida. Eles aparecem por padrão na lista de ambiente do [espaço de trabalho](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29), com o prefixo "AzureML".

  + **azureml-Train-automl**
  + **[azureml-Train-automl](/python/api/azureml-train-automl-runtime/)**
    + Adicionado o suporte de conversão de ONNX para o ADB e o HDI

+ **Recursos de visualização**
  + **azureml-Train-automl**
  + **[azureml-Train-automl](/python/api/azureml-train-automl-runtime/)**
    + BERT e BiLSTM com suporte como Text featurizer (somente visualização)
    + Personalização de personalização com suporte para parâmetros de finalidade de coluna e transformador (somente visualização)
    + Explicações brutas com suporte quando o usuário habilita a explicação do modelo durante o treinamento (somente visualização)
    + Prophet adicionado para `timeseries` previsão como um pipeline treinado (somente visualização)

  + **azureml-contrib-descompasso**
    + Pacotes realocados de azureml-contrib-datalogoff para azureml-datalogoff; o `contrib` pacote será removido em uma versão futura

+ **Correções de bugs e melhorias**
  + **azureml-automl-core**
    + Introduziu o FeaturizationConfig em AutoMLConfig e AutoMLBaseSettings
    + Introduziu o FeaturizationConfig em [AutoMLConfig](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) e AutoMLBaseSettings
      + Substituir a finalidade da coluna para personalização com a coluna especificada e o tipo de recurso
      + Substituir parâmetros do transformador
    + Mensagem de substituição adicionada para explain_model () e retrieve_model_explanations ()
    + Prophet adicionado como um pipeline treinado (somente visualização)
    + Mensagem de substituição adicionada para explain_model () e retrieve_model_explanations ().
    + Prophet adicionado como um pipeline treinado (somente visualização).
    + Suporte adicionado para detecção automática de atraso de destino, tamanho de janela sem interrupção e horizonte máximo. Se um dos target_lags, target_rolling_window_size ou max_horizon for definido como ' auto ', a heurística será aplicada para estimar o valor do parâmetro correspondente com base nos dados de treinamento.
    + Correção de previsão no caso em que o conjunto de dados contém uma coluna de refinamento, essa granulação é de um tipo numérico e há uma lacuna entre o treinamento e o conjunto de teste
    + Correção da mensagem de erro sobre o índice duplicado na execução remota em tarefas de previsão
    + Correção de previsão no caso em que o conjunto de dados contém uma coluna de refinamento, essa granulação é de um tipo numérico e há uma lacuna entre o treinamento e o conjunto de teste.
    + Correção da mensagem de erro sobre o índice duplicado na execução remota em tarefas de previsão.
    + Adicionou um Guardrail para verificar se um conjunto de um DataSet está desequilibrado ou não. Se for, uma mensagem Guardrail seria gravada no console.
  + **azureml-núcleo**
    + Foi adicionada a capacidade de recuperar a URL da SAS para o modelo no armazenamento por meio do objeto de modelo. Ex.: Model. get_sas_url ()
    + Introduzir `run.get_details()['datasets']` para obter conjuntos de valores associados à execução enviada
    + Adicione a `Dataset.Tabular.from_json_lines_files` API para criar um TabularDataset de arquivos de linhas JSON. Para saber mais sobre esses dados tabulares em arquivos de linhas JSON em TabularDataset https://aka.ms/azureml-data , visite para obter a documentação.
    + Adição de campos de tamanho de VM adicionais (disco do so, número de GPUs) à função supported_vmsizes ()
    + Adicionados campos adicionais à função list_nodes () para mostrar a execução, o IP privado e o público, a porta, etc.
    + Capacidade de especificar um novo campo durante o provisionamento de cluster--remotelogin_port_public_access que pode ser definido como habilitado ou desabilitado, dependendo se você deseja deixar a porta SSH aberta ou fechada no momento da criação do cluster. Se você não especificá-lo, o serviço abrirá de forma inteligente ou fechará a porta dependendo se você estiver implantando o cluster dentro de uma VNet.
  + **azureml-explain-model**
  + **[azureml-núcleo](https://docs.microsoft.com/python/api/azureml-core/azureml.core)**
    + Foi adicionada a capacidade de recuperar a URL da SAS para o modelo no armazenamento por meio do objeto de modelo. Por exemplo: modelo. [get_sas_url ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model#get-sas-urls--)
    + Introduza executar. [get_details](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--)[' DataSets '] para obter conjuntos de valores associados à execução enviada
    + Adicionar API `Dataset.Tabular`. [from_json_lines_files ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory#from-json-lines-files-path--validate-true--include-path-false--set-column-types-none--partition-format-none-) para criar um TabularDataset de arquivos de linhas JSON. Para saber mais sobre esses dados tabulares em arquivos de linhas JSON em TabularDataset https://aka.ms/azureml-data , visite para obter a documentação.
    + Adição de campos de tamanho de VM adicionais (disco do so, número de GPUs) à função [supported_vmsizes ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#supported-vmsizes-workspace--location-none-)
    + Adicionados campos adicionais à função [list_nodes ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#list-nodes--) para mostrar a execução, o IP privado e o público, a porta, etc.
    + Capacidade de especificar um novo campo durante `--remotelogin_port_public_access` o [provisionamento](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--) de cluster que pode ser definido como habilitado ou desabilitado, dependendo se você deseja deixar a porta SSH aberta ou fechada no momento da criação do cluster. Se você não especificá-lo, o serviço abrirá de forma inteligente ou fechará a porta dependendo se você estiver implantando o cluster dentro de uma VNet.
  + **azureml-explain-model**
    + Documentação aprimorada para obter as saídas de explicação no cenário de classificação.
    + Adicionada a capacidade de carregar os valores y previstos na explicação para os exemplos de avaliação. O desbloqueia visualizações mais úteis.
    + Adicionada a propriedade de explicador para MimicWrapper para habilitar a obtenção do MimicExplainer subjacente.
  + **azureml-pipeline-Core**
    + Bloco de anotações adicionado para descrever o módulo, ModuleVersion e ModuleStep
  + **azureml-pipeline-steps**
    + Adição de RScriptStep para dar suporte ao script R executado via pipeline AML.
    + Parâmetros de metadados corrigidos analisando em AzureBatchStep que estava causando a mensagem de erro "a atribuição para o parâmetro SubscriptionId não foi especificada".
  + **azureml-Train-automl**
    + Training_data com suporte, validation_data, label_column_name weight_column_name como formato de entrada de dados
    + Mensagem de substituição adicionada para explain_model () e retrieve_model_explanations ()
  + **[azureml-pipeline-Core](https://docs.microsoft.com/python/api/azureml-pipeline-core)**
    + Adicionado um [bloco de anotações](https://aka.ms/pl-modulestep) para descrever [Module](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.module(class)), [ModuleVersion](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.moduleversion) e [ModuleStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.modulestep).
  + **[azureml-pipeline-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps)**
    + Adição de [RScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.rscriptstep) para dar suporte ao script R executado via pipeline AML.
    + Parâmetros de metadados corrigidos analisando em [AzureBatchStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.azurebatchstep) que estava causando a mensagem de erro "a atribuição para o parâmetro SubscriptionId não foi especificada".
  + **[azureml-Train-automl](/python/api/azureml-train-automl-runtime/)**
    + Training_data com suporte, validation_data, label_column_name weight_column_name como formato de entrada de dados.
    + Mensagem de substituição adicionada para [explain_model ()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#explain-model-fitted-model--x-train--x-test--best-run-none--features-none--y-train-none----kwargs-) e [retrieve_model_explanations ()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#retrieve-model-explanation-child-run-).


## <a name="2019-09-16"></a>2019-09-16

### <a name="azure-machine-learning-sdk-for-python-v1062"></a>Azure Machine Learning SDK para Python v 1.0.62

+ **Novos recursos**
  + Introduziu `timeseries` a característica em TabularDataset. Essa característica permite uma fácil filtragem de carimbo de data/hora nos dados de uma TabularDataset, como a obtenção de todos os dados entre um intervalo de tempo ou os dados mais recentes. Para saber mais sobre essa `timeseries` característica em TabularDataset, visite https://aka.ms/azureml-data a documentação ou https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb para obter um exemplo de bloco de anotações.
  + Habilitado o treinamento com TabularDataset e filedataset. Visite https://aka.ms/dataset-tutorial um exemplo de bloco de anotações.

  + **azureml-train-core**
      + Adição `Nccl` e `Gloo` suporte no estimador de PyTorch

+ **Correções de bugs e melhorias**
  + **azureml-automl-core**
    + Preterida a configuração AutoML ' lag_length ' e LaggingTransformer.
    + Correção correta da validação de dados de entrada se elas forem especificadas em um formato de Dataflow
    + Modificou o fit_pipeline. py para gerar o grafo JSON e carregar em artefatos.
    + Renderizado o grafo `userrun` em `Cytoscape`usando.
  + **azureml-núcleo**
    + Revisitoumos o tratamento de exceção no código ADB e fazemos alterações de acordo com o tratamento de erro por nova
    + Autenticação MSI automática adicionada para VMs do notebook.
    + Corrige o bug em que modelos corrompidos ou vazios podem ser carregados devido a tentativas com falha.
    + Corrigido o bug em `DataReference` que o nome é `DataReference` alterado quando o modo é alterado ( `as_upload`por `as_download`exemplo, `as_mount`ao chamar,, ou).
    + Make `mount_point` e `target_path` optional para `FileDataset.mount` e `FileDataset.download`.
    + A exceção de que a coluna de carimbo de data/hora não pode ser encontrada será lançada se a API relacionada à hora de série for chamada sem a coluna de carimbo de data/hora correta atribuída ou as colunas de carimbo de data/hora
    + As colunas de série de tempo devem ser atribuídas com a coluna cujo tipo é Date, caso contrário a exceção é esperada
    + As colunas de série de tempo atribuindo a API ' with_timestamp_columns ' pode não receber nenhum valor/nome de coluna de carimbo de data/hora grande, o que limpará as colunas de carimbo de data/hora atribuídas
    + A exceção será gerada quando a coluna de carimbo de data/hora de alta granularidade ou refinada for descartada com a indicação para o usuário que o descarte pode ser feito após a exclusão da coluna de carimbo de data/hora na lista suspensa ou chamada with_time_stamp com nenhum valor para liberar colunas timestamp
    + A exceção será gerada quando a coluna de carimbo de data/hora de alta granularidade ou refinada não estiver incluída na lista manter colunas com a indicação para o usuário que pode ser realizada após a inclusão da coluna timestamp na lista manter coluna ou chamar with_time_stamp com nenhum valor para liberar colunas timestamp.
    + Adicionado log para o tamanho de um modelo registrado.
  + **azureml-explain-model**
    + Correção de aviso impresso no console quando o pacote do Python "empacotamento" não está instalado: "usando a versão mais antiga do que com suporte do lightgbm, atualize para a versão superior à 2.2.1"
    + Corrigida a explicação do modelo de download com a fragmentação para obter explicações globais com muitos recursos
    + O explicador de imitação corrigida não contém exemplos de inicialização na explicação de saída
    + Corrigido o erro imutável nas propriedades definidas ao carregar com o cliente de explicação usando dois tipos diferentes de modelos
    + Adição de um parâmetro de get_raw ao explicador de pontuação. explique () para que um explicador de Pontuação possa retornar valores de engenharia e brutos.
  + **azureml-Train-automl**
    + Introduziu as APIs públicas do AutoML para obter `automl` explicações de suporte de explicar o SDK – uma maneira mais recente de dar suporte a explicações de AutoML ao desacoplar AutoML personalização e explicar o suporte à explicação bruta integrada ao SDK do AZUREML explicar SDK para modelos de AutoML.
    + Removendo o azureml-padrões de ambientes de treinamento remoto.
    + Local do repositório de cache padrão alterado de FileCacheStore com base em um para AzureFileCacheStore um para AutoML no caminho de código Azure Databricks.
    + Correção correta da validação de dados de entrada se elas forem especificadas em um formato de Dataflow
  + **azureml-train-core**
    + Revertida source_directory_data_store substituição.
    + Foi adicionada a capacidade de substituir as versões de pacote instaladas do azureml.
    + Adicionado suporte a dockerfile `environment_definition` no parâmetro em estimadores.
    + Parâmetros de treinamento distribuídos simplificados em estimadores.

         ```py
        from azureml.train.dnn import TensorFlow, Mpi, ParameterServer
        ```

## <a name="2019-09-09"></a>2019-09-09

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Nova experiência na Web (versão prévia) para espaços de trabalho Azure Machine Learning
A nova experiência na Web permite que cientistas de dados e engenheiros de dados concluam seu ciclo de vida de aprendizado de máquina de ponta a ponta de preparando e visualizando dados para treinar e implantar modelos em um único local.

![Interface do usuário do Azure Machine Learning Workspace (versão prévia)](./media/azure-machine-learning-release-notes/new-ui-for-workspaces.jpg)

**Principais recursos:**

Usando essa nova interface de Azure Machine Learning, agora você pode:
+ Gerencie seus notebooks ou conecte-se ao Jupyter
+ [Executar experimentos de ML automatizados](tutorial-first-experiment-automated-ml.md)
+ [Criar conjuntos de itens de arquivos locais, repositórios de armazenamento & arquivos da Web](how-to-create-register-datasets.md)
+ Explorar & preparar conjuntos de valores para a criação do modelo
+ Monitorar descompasso de dados para seus modelos
+ Exibir os recursos recentes de um painel

No momento desta versão, há suporte para os seguintes navegadores: Chrome, Firefox, Safari e Microsoft Edge Preview.

**Problemas conhecidos:**

1. Atualize seu navegador se você vir "algo deu errado! Erro ao carregar arquivos de parte "quando a implantação está em andamento.

1. Não é possível excluir ou renomear o arquivo em arquivos e blocos de anotações. Durante a visualização pública, você pode usar a interface do usuário do Jupyter ou terminal na VM do notebook para executar operações de atualização de arquivo. Como é um sistema de arquivos de rede montado, todas as alterações feitas na VM do notebook são refletidas imediatamente no espaço de trabalho do bloco de anotações.

1. Para SSH na VM do bloco de anotações:
   1. Localize as chaves SSH que foram criadas durante a configuração da VM. Ou localize as chaves no espaço de trabalho Azure Machine Learning > guia abrir computação > localizar a VM do bloco de anotações na lista > abra suas propriedades: copie as chaves da caixa de diálogo.
   1. Importe as chaves SSH públicas e privadas para o computador local.
   1. Use-os para SSH na VM do notebook.

## <a name="2019-09-03"></a>2019-09-03
### <a name="azure-machine-learning-sdk-for-python-v1060"></a>Azure Machine Learning SDK para Python v 1.0.60

+ **Novos recursos**
  + Introduziu filedataset, que faz referência a um ou vários arquivos em seus armazenamentos de dados ou URLs públicas. Os arquivos podem ser de qualquer formato. O filedataset fornece a capacidade de baixar ou montar os arquivos em sua computação. Para saber mais sobre filedataset, visite https://aka.ms/file-dataset.
  + Adicionado suporte de YAML de pipeline para a etapa PythonScript, etapa adla, databricks etapa, DataTransferStep e etapa AzureBatch

+ **Correções de bugs e melhorias**
  + **azureml-automl-core**
    + AutoArima agora é um pipeline sugerido para visualização apenas.
    + Relatório de erros aprimorado para previsão.
    + Melhorou o registro em log usando exceções personalizadas em vez de genéricas nas tarefas de previsão.
    + Foi removido o check-in max_concurrent_iterations ser menor que o número total de iterações.
    + Os modelos AutoML agora retornam AutoMLExceptions
    + Esta versão melhora o desempenho de execução das execuções locais do Machine Learning automatizado.
  + **azureml-núcleo**
    + Introduza DataSet. get_all (Workspace), que retorna um dicionário de `TabularDataset` e `FileDataset` objetos com chave por seu nome de registro.

    ```py
    workspace = Workspace.from_config()
    all_datasets = Dataset.get_all(workspace)
    mydata = all_datasets['my-data']
    ```

    + Introduza `parition_format` como argumento para `Dataset.Tabular.from_delimited_files` e `Dataset.Tabular.from_parquet.files`. As informações de partição de cada caminho de dados serão extraídas em colunas com base no formato especificado. ' {column_name} ' cria uma coluna de cadeia de caracteres e ' {column_name: YYYY/MM/DD/HH/mm/ss} ' cria a coluna DateTime, onde ' yyyy ', ' MM ', ' dd ', ' HH ', ' mm ' e ' ss ' são usados para extrair ano, mês, dia, hora, minuto e segundo para o tipo DateTime. O partition_format deve começar da posição da primeira chave de partição até o final do caminho do arquivo. Por exemplo, considerando o caminho '.. /USA/2019/01/01/data.csv ', em que a partição é por país e hora, partition_format = '/{Country}/{PartitionDate: YYYY/MM/DD}/Data. csv ' cria a coluna de cadeia de caracteres ' Country ' com o valor ' EUA ' e a coluna DateTime ' PartitionDate ' com o valor ' 2019-01-01 '.
        ```py
        workspace = Workspace.from_config()
        all_datasets = Dataset.get_all(workspace)
        mydata = all_datasets['my-data']
        ```

    + Introduza `partition_format` como argumento para `Dataset.Tabular.from_delimited_files` e `Dataset.Tabular.from_parquet.files`. As informações de partição de cada caminho de dados serão extraídas em colunas com base no formato especificado. ' {column_name} ' cria uma coluna de cadeia de caracteres e ' {column_name: YYYY/MM/DD/HH/mm/ss} ' cria a coluna DateTime, onde ' yyyy ', ' MM ', ' dd ', ' HH ', ' mm ' e ' ss ' são usados para extrair ano, mês, dia, hora, minuto e segundo para o tipo DateTime. O partition_format deve começar da posição da primeira chave de partição até o final do caminho do arquivo. Por exemplo, considerando o caminho '.. /USA/2019/01/01/data.csv ', em que a partição é por país e hora, partition_format = '/{Country}/{PartitionDate: YYYY/MM/DD}/Data. csv ' cria a coluna de cadeia de caracteres ' Country ' com o valor ' EUA ' e a coluna DateTime ' PartitionDate ' com o valor ' 2019-01-01 '.
    + `to_csv_files`e `to_parquet_files` foram adicionados métodos ao `TabularDataset`. Esses métodos habilitam a conversão `TabularDataset` entre um `FileDataset` e um convertendo os dados em arquivos do formato especificado.
    + Faça logon automaticamente no registro da imagem base ao salvar um Dockerfile gerado por Model. Package ().
    + ' gpu_support ' não é mais necessário; O AML agora detecta e usa automaticamente a extensão do Docker NVIDIA quando ela está disponível. Ela será removida em uma versão futura.
    + Adicionado suporte para criar, atualizar e usar PipelineDrafts.
    + Esta versão melhora o desempenho de execução das execuções locais do Machine Learning automatizado.
    + Os usuários podem consultar métricas do histórico de execuções por nome.
    + Melhorou o registro em log usando exceções personalizadas em vez de genéricas nas tarefas de previsão.
  + **azureml-explain-model**
    + Adicionado feature_maps parâmetro ao novo MimicWrapper, permitindo que os usuários obtenham explicações de recursos brutos.
    + Os carregamentos de conjunto de ativos agora estão desativados por padrão para o upload de explicação e podem ser habilitados novamente com upload_datasets = true
    + Os parâmetros de filtragem "is_law" foram adicionados à lista de explicação e às funções de download.
    + Adiciona o `get_raw_explanation(feature_maps)` método a objetos de explicação global e local.
    + Adicionada a verificação de versão para lightgbm com aviso impresso, se abaixo da versão com suporte
    + Uso otimizado de memória no envio em lote de explicações
    + Os modelos AutoML agora retornam AutoMLExceptions
  + **azureml-pipeline-Core**
    + Suporte adicionado para criar, atualizar e usar PipelineDrafts-pode ser usado para manter definições de pipeline mutáveis e usá-las interativamente para executar
  + **azureml-Train-automl**
    + Foi criado um recurso para instalar versões específicas de pytorch v 1.1.0, :::no-loc text="cuda"::: Toolkit 9,0, pytorch-transformadores, que é necessário para habilitar o Bert/XLNet no ambiente de tempo de execução do Python remoto.
  + **azureml-train-core**
    + Falha antecipada de alguns erros de definição de espaço de hiperparâmetro diretamente no SDK, em vez de no lado do servidor.

### <a name="azure-machine-learning-data-prep-sdk-v1114"></a>SDK v 1.1.14 do Azure Machine Learning data Prep
+ **Correções de bugs e melhorias**
  + Habilitado gravação em ADLS/ADLSGen2 usando caminho e credenciais brutos.
  + Corrigido um bug que causou `include_path=True` o trabalho de não `read_parquet`funcionar para o.
  + Correção `to_pandas_dataframe()` da falha causada pela exceção "valor da propriedade inválido: hostSecret".
  + Correção de um bug em que os arquivos não puderam ser lidos em DBFS no modo Spark.

## <a name="2019-08-19"></a>2019-08-19

### <a name="azure-machine-learning-sdk-for-python-v1057"></a>Azure Machine Learning SDK para Python v 1.0.57
+ **Novos recursos**
  + Habilitado `TabularDataset` para ser consumido pelo AutomatedML. Para saber mais sobre `TabularDataset`o, visite https://aka.ms/azureml/howto/createdatasets.

+ **Correções de bugs e melhorias**
  + **automl-Client-Core-nativeClient**
    + O erro foi corrigido, gerado quando os rótulos de treinamento e/ou de validação (y e y_valid) são fornecidos na forma de dataframe do pandas, mas não como matriz numpy.
    + Interface atualizada para criar um `RawDataContext` para exigir apenas os dados e o `AutoMLBaseSettings` objeto.
    +  Permitir que os usuários do AutoML descartam a série de treinamento que não são longas o suficiente ao prever. -Permitir que usuários do AutoML removam granulares do conjunto de teste que não existe no conjunto de treinamento ao prever.
  + **azure-cli-ml**
    + Agora você pode atualizar o certificado TLS/SSL para o ponto de extremidade de Pontuação implantado no cluster AKS para o Microsoft generated e o certificado do cliente.
  + **azureml-automl-core**
    + Corrigido um problema em AutoML em que as linhas com rótulos ausentes não foram removidas corretamente.
    + Aprimoramento do log de erros no AutoML; as mensagens de erro completas agora serão sempre gravadas no arquivo de log.
    + O AutoML atualizou sua fixação de pacote `azureml-defaults`para `azureml-explain-model`incluir, `azureml-dataprep`e. O AutoML não avisará mais sobre incompatibilidades de pacotes ( `azureml-train-automl` exceto o pacote).
    + Corrigido um problema em `timeseries` que as divisões de CV são de tamanho diferente, causando a falha do cálculo de compartimento.
    + Ao executar a iteração de Ensemble para o tipo de treinamento de validação cruzada, se acabarmos tendo problemas para baixar os modelos treinados em todo o conjunto de ativos, estamos tendo uma inconsistência entre os pesos do modelo e os modelos que estavam sendo inseridos no Ensemble de votação.
    + O erro foi corrigido, gerado quando os rótulos de treinamento e/ou de validação (y e y_valid) são fornecidos na forma de dataframe do pandas, mas não como matriz numpy.
    + Corrigido o problema com as tarefas de previsão quando nenhuma foi encontrada nas colunas booleanas das tabelas de entrada.
    + Permitir que os usuários do AutoML descartam a série de treinamento que não são longas o suficiente ao prever. -Permitir que usuários do AutoML removam granulares do conjunto de teste que não existe no conjunto de treinamento ao prever.
  + **azureml-núcleo**
    + Correção do problema com blob_cache_timeout ordenação de parâmetros.
    + Adicionados tipos de exceção de ajuste externo e de transformação a erros do sistema.
    + Suporte adicionado para segredos de Key Vault para execuções remotas. Adicione uma classe azureml. Core. keyvault. keyvault para adicionar, obter e listar segredos do keyvault associado ao seu espaço de trabalho. As operações com suporte são:
      + azureml. Core. Workspace. Workspace. get_default_keyvault ()
      + azureml. Core. keyvault. keyvault. set_secret (nome, valor)
      + azureml. Core. keyvault. keyvault. set_secrets (secrets_dict)
      + azureml. Core. keyvault. keyvault. get_secret (Name)
      + azureml. Core. keyvault. keyvault. get_secrets (secrets_list)
      + azureml. Core. keyvault. keyvault. list_secrets ()
    + Métodos adicionais para obter o keyvault padrão e obter segredos durante a execução remota:
      + azureml. Core. Workspace. Workspace. get_default_keyvault ()
      + azureml. Core. Run. Run. get_secret (Name)
      + azureml. Core. Run. Run. get_secrets (secrets_list)
    + Foram adicionados parâmetros adicionais de substituição para o comando de CLI enviar-hyperdrive.
    + Melhorar a confiabilidade das chamadas de API, expandindo repetições para exceções de biblioteca de solicitações comuns.
    + Adicione suporte para envio de execuções de uma execução enviada.
    + Corrigido o problema de token SAS de expiração no arquivo de monitoramento, que fez com que os arquivos pararam de ser carregados depois que seu token inicial tivesse expirado.
    + Suporte à importação de arquivos HTTP CSV/TSV no SDK do Python do conjunto de
    + O método Workspace. Setup () foi substituído. A mensagem de aviso mostrada aos usuários sugere o uso de Create () ou Get ()/from_config () em vez disso.
    + Adicionado Environment. add_private_pip_wheel (), que permite carregar pacotes `whl`Python personalizados privados no espaço de trabalho e usá-los com segurança para criar/materializar o ambiente.
    + Agora você pode atualizar o certificado TLS/SSL para o ponto de extremidade de Pontuação implantado no cluster AKS para o Microsoft generated e o certificado do cliente.
  + **azureml-explain-model**
    + Parâmetro adicionado para adicionar uma ID de modelo a explicações no carregamento.
    + Marcação `is_raw` adicionada a explicações na memória e no carregamento.
    + Adicionado suporte e testes do pytorch para o pacote azureml-explique-Model.
  + **azureml-opendatasets**
    + Suporte para detectar e registrar em log o ambiente de teste automático.
    + Classes adicionadas para nos colocar população por município e zip.
  + **azureml-pipeline-Core**
    + Adicionada a propriedade de rótulo às definições de porta de entrada e saída.
  + **azureml – telemetria**
    + Correção de configuração de telemetria incorreta.
  + **azureml-Train-automl**
    + Correção do bug em que ocorreu uma falha na instalação, o erro não estava sendo registrado no campo "erros" para a execução da instalação e, portanto, não foi armazenado em "erros" da execução do pai.
    + Corrigido um problema em AutoML em que as linhas com rótulos ausentes não foram removidas corretamente.
    + Permitir que os usuários do AutoML descartam a série de treinamento que não são longas o suficiente ao prever.
    + Permita que os usuários do AutoML removam granulares do conjunto de teste que não existem no conjunto de treinamento ao prever.
    + Agora, o AutoMLStep `automl` passa pela configuração para o back-end para evitar problemas de alterações ou adições de novos parâmetros de configuração.
    + O AutoML data Guardrail agora está em visualização pública. O usuário verá um relatório de guardrail de dados (para tarefas de classificação/regressão) após o treinamento e também poderá acessá-lo por meio da API do SDK.
  + **azureml-train-core**
    + Adicionado suporte do Torch 1,2 no estimador do PyTorch.
  + **azureml-widgets**
    + Gráficos de matriz de confusão aprimorados para treinamento de classificação.

### <a name="azure-machine-learning-data-prep-sdk-v1112"></a>SDK v 1.1.12 do Azure Machine Learning data Prep
+ **Novos recursos**
  + Listas de cadeias de caracteres agora podem ser passadas `read_*` como entrada para métodos.

+ **Correções de bugs e melhorias**
  + O desempenho do `read_parquet` foi significativamente melhorado quando executado no Spark.
  + Corrigido um problema em `column_type_builder` que houve falha no caso de uma única coluna com formatos de data ambíguos.

### <a name="azure-portal"></a>Portal do Azure
+ **Recurso de visualização**
  + O streaming do arquivo de log e saída agora está disponível para páginas de detalhes de execução. Os arquivos transmitirão atualizações em tempo real quando a alternância de visualização estiver ativada.
  + A capacidade de definir a cota em um nível de espaço de trabalho é liberada na versão prévia. As cotas de AmlCompute são alocadas no nível da assinatura, mas agora podemos distribuir essa cota entre espaços de trabalho e alocá-la para compartilhamento e governança justas. Basta clicar na folha **usos + cotas** na barra de navegação à esquerda do espaço de trabalho e selecionar a guia **configurar cotas** . Observe que você deve ser um administrador de assinatura para poder definir cotas no nível do espaço de trabalho, uma vez que essa é uma operação entre espaços de trabalho.

## <a name="2019-08-05"></a>2019-08-05

### <a name="azure-machine-learning-sdk-for-python-v1055"></a>Azure Machine Learning SDK para Python v 1.0.55

+ **Novos recursos**
  + A autenticação baseada em token agora tem suporte para as chamadas feitas para o ponto de extremidade de Pontuação implantado em AKS. Continuaremos a dar suporte à autenticação baseada em chave atual e os usuários podem usar um desses mecanismos de autenticação de cada vez.
  + Capacidade de registrar um armazenamento de BLOBs que está atrás da rede virtual (VNet) como um repositório de armazenamento.

+ **Correções de bugs e melhorias**
  + **azureml-automl-core**
    + Corrige um bug em que o tamanho de validação para as divisões de CV é pequeno e resulta em gráficos de previsão inválidas versus reais para regressão e previsões.
    + O registro em log de tarefas de previsão nas execuções remotas é melhorado. agora, o usuário receberá uma mensagem de erro abrangente se a execução tiver falhado.
    + Correção de falhas `Timeseries` de se o sinalizador de pré-processamento for verdadeiro.
    + Foram feitas algumas mensagens de erro de validação de dados de previsão mais acionáveis.
    + Redução do consumo de memória das execuções de AutoML ao descartar e/ou carregar o carregamento lento de conjuntos de valores, especialmente entre as execuções de processo
  + **azureml-contrib-explicação-modelo**
    + Foi adicionado model_task sinalizador aos explicadores para permitir que o usuário substitua a lógica de inferência automática padrão para o tipo de modelo
    + Alterações do widget: o instala `contrib`automaticamente com o `nbextension` , não mais explicação de instalação/habilitação-suporte com apenas a importância do recurso global (por exemplo, permutal)
    + Alterações no painel: plotagens de caixa e gráficos violino além de `beeswarm` plotar na página de resumo-reprocessamento muito mais rápido `beeswarm` de plotagem no controle deslizante ' top-k '-mensagem útil explicando como as primeiras k são mensagens personalizáveis úteis computadas no lugar de gráficos quando os dados não são fornecidos
  + **azureml-núcleo**
    + Adicionado o método Model. Package () para criar imagens do Docker e Dockerfiles que encapsulam modelos e suas dependências.
    + Os WebServices locais foram atualizados para aceitar o InferenceConfigs que contém objetos de ambiente.
    + Corrigido o modelo. Register () produzindo modelos inválidos quando '. ' (para o diretório atual) é passado como o parâmetro model_path.
    + Adicionar Run. submit_child, a funcionalidade espelha o experimento. Submit ao especificar a execução como pai da execução filho enviada.
    + Suporte a opções de configuração do modelo. Register em Run. register_model.
    + Capacidade de executar trabalhos JAR no cluster existente.
    + Agora oferecendo suporte a parâmetros instance_pool_id e cluster_log_dbfs_path.
    + Adicionado suporte para usar um objeto de ambiente ao implantar um modelo em um WebService. O objeto de ambiente agora pode ser fornecido como parte do objeto InferenceConfig.
    + Adicionar mapeamento de appinsifht para novas regiões-centralus-oesteus-northcentralus
    + Adicionada documentação para todos os atributos em todas as classes de armazenamento de Datastore.
    + Adicionado blob_cache_timeout parâmetro a `Datastore.register_azure_blob_container`.
    + Foram adicionados save_to_directory e métodos load_from_directory ao azureml. Core. Environment. Environment.
    + Foram adicionados os comandos "AZ ml Environment download" e "AZ ml Environment Register" à CLI.
    + Adicionado o método Environment. add_private_pip_wheel.
  + **azureml-explain-model**
    + Adicionado controle de conjunto de alterações a explicações usando o serviço de conjunto de serviços (versão prévia).
    + Tamanho de lote padrão reduzido ao transmitir explicações globais de 10K para 100.
    + Foi adicionado model_task sinalizador aos explicadores para permitir que o usuário substitua a lógica de inferência automática padrão para o tipo de modelo.
  + **azureml-mlflow**
    + Corrigido o bug em mlflow. azureml. build_image em que os diretórios aninhados são ignorados.
  + **azureml-pipeline-steps**
    + Foi adicionada a capacidade de executar trabalhos JAR no cluster Azure Databricks existente.
    + Adicionado instance_pool_id de suporte e parâmetros de cluster_log_dbfs_path para a etapa DatabricksStep.
    + Adicionado suporte para parâmetros de pipeline na etapa DatabricksStep.
  + **azureml-Train-automl**
    + Adicionado `docstrings` para os arquivos relacionados ao Ensemble.
    + Documentos atualizados para um idioma mais apropriado `max_cores_per_iteration` para o e o`max_concurrent_iterations`
    + O registro em log de tarefas de previsão nas execuções remotas é melhorado. agora, o usuário receberá uma mensagem de erro abrangente se a execução tiver falhado.
    + Get_data removido do bloco `automlstep` de anotações do pipeline.
    + Iniciado o `dataprep` suporte `automlstep`no.

### <a name="azure-machine-learning-data-prep-sdk-v1110"></a>SDK v 1.1.10 do Azure Machine Learning data Prep

+ **Novos recursos**
  + Agora você pode solicitar para executar inspetores específicos (por exemplo, histograma, gráfico de dispersão, etc.) em colunas específicas.
  + Adicionado um argumento paralelizate a `append_columns`. Se for true, os dados serão carregados na memória, mas a execução será executada em paralelo; Se for false, a execução será transmitida, mas de thread único.

## <a name="2019-07-23"></a>2019-07-23

### <a name="azure-machine-learning-sdk-for-python-v1053"></a>Azure Machine Learning SDK para Python v 1.0.53

+ **Novos recursos**
  + O Machine Learning automatizado agora dá suporte a modelos de ONNX de treinamento no destino de computação remota
  + Azure Machine Learning agora fornece a capacidade de retomar o treinamento de uma execução anterior, um ponto de verificação ou arquivos de modelo.
    + Saiba como [usar os estimadores para retomar o treinamento de uma execução anterior](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-tensorflow-resume-training/train-tensorflow-resume-training.ipynb)

+ **Correções de bugs e melhorias**
  + **automl-Client-Core-nativeClient**
    + Corrigir o bug sobre os tipos de colunas perder após a transformação (bug vinculado);
    + Permitir que y_query seja um tipo de objeto que contenha nenhum (s) no início (#459519).
  + **azure-cli-ml**
    + Os comandos da CLI "implantação de modelo" e "atualização de serviço" agora aceitam parâmetros, arquivos de configuração ou uma combinação dos dois. Parâmetros têm precedência sobre atributos em arquivos.
    + A descrição do modelo agora pode ser atualizada após o registro
  + **azureml-automl-core**
    + Atualize a dependência NimbusML para a versão 1.2.0 (atual mais recente).
    + Adicionar suporte para NimbusML estimados & pipelines a serem usados em estimadores AutoML.
    + Corrigindo um bug no procedimento de seleção de Ensemble que estava aumentando desnecessariamente o Ensemble resultante, mesmo se as pontuações permaneceram constantes.
    + Habilite a reutilização de algumas featurizations em divisões de CV para tarefas de previsão. Isso acelera o tempo de execução da configuração executada por aproximadamente um fator de n_cross_validations para featurizations caras, como janelas atrasadas e sem interrupção.
    + Resolver um problema se o tempo estiver fora do intervalo de tempo com suporte do pandas. Agora, geramos uma DataException se o tempo for menor que PD. Timestamp. min ou maior que PD. Timestamp. Max
    + A previsão agora permite diferentes frequências em conjuntos de treinamento e teste se eles puderem ser alinhados. Por exemplo, "começando trimestralmente em Janeiro" e em "a partir de outubro", a cada trimestre, pode ser alinhado.
    + A propriedade "Parameters" foi adicionada ao TimeSeriesTransformer.
    + Remova as classes de exceção antigas.
    + Em tarefas de previsão, o `target_lags` parâmetro agora aceita um único valor inteiro ou uma lista de inteiros. Se o inteiro tiver sido fornecido, apenas uma latência será criada. Se uma lista for fornecida, os valores exclusivos de retardo serão feitos. target_lags = [1, 2, 2, 4] criará um retardo de um, 2 e 4 períodos.
    + Corrigir o bug sobre a perda de tipos de colunas após a transformação (bug vinculado);
    + No `model.forecast(X, y_query)`, permitir que y_query seja um tipo de objeto que contém nenhum (s) no início (#459519).
    + Adicionar valores esperados `automl` à saída
  + **azureml-contrib-descompasso**
    +  Melhorias no notebook de exemplo, incluindo alternar para o azureml-opendatasets em vez de azureml-contrib-opendatasets e melhorias de desempenho ao aprimorar dados
  + **azureml-contrib-explicação-modelo**
    + Argumento de transformações fixas para o explicador de verde-limão para importância de recurso bruto no azureml-contrib-explique-Package
    + Segmentações adicionadas a explicações de imagem no explicador de imagem para o pacote AzureML-contrib-explique-Model
    + Adicionar suporte esparso scipy para LimeExplainer
    + Adicionado `batch_size` ao explicador de imitação quando `include_local=False`, para transmitir explicações globais em lotes para melhorar o tempo de execução de DecisionTreeExplainableModel
  + **azureml-contrib-featureengineering**
    + Correção para chamar set_featurizer_timeseries_params (): tipo de valor de dict Change e verificação nula-adicionar `timeseries` bloco de anotações para featurizer
    + Atualize a dependência NimbusML para a versão 1.2.0 (atual mais recente).
  + **azureml-núcleo**
    + Adicionada a capacidade de anexar armazenamentos de DBFS na CLI do AzureML
    + Corrigido o bug com o carregamento do repositório de armazenamento em que uma pasta `target_path` vazia é criada, se iniciada com`/`
    + Corrigido `deepcopy` o problema em ServicePrincipalAuthentication.
    + Adicionados os comandos "AZ ml Environment show" e "AZ ml Environment List" à CLI.
    + Os ambientes agora dão suporte à especificação de um base_dockerfile como uma alternativa a um base_image já criado.
    + A configuração RunConfiguration não utilizada auto_prepare_environment foi marcada como preterida.
    + A descrição do modelo agora pode ser atualizada após o registro
    + Bugfix: o modelo e a exclusão de imagem agora fornecem mais informações sobre como recuperar objetos upstream que dependem deles se a exclusão falhar devido a uma dependência upstream.
    + Correção do bug que imprimiu a duração em branco para implantações que ocorrem ao criar um espaço de trabalho para alguns ambientes.
    + Melhores exceções de falha na criação do espaço de trabalho. De modo que os usuários não vejam "não é possível criar o espaço de trabalho. Não é possível localizar... " como a mensagem e, em vez disso, veja a falha de criação real.
    + Adicione suporte para autenticação de token em WebServices AKS.
    + Adicionar `get_token()` método a `Webservice` objetos.
    + Adicionado suporte à CLI para gerenciar os conjuntos de computadores de aprendizado de máquina.
    + `Datastore.register_azure_blob_container`Agora, opcionalmente, `blob_cache_timeout` usa um valor (em segundos) que configura os parâmetros de montagem do blobfuse para habilitar a expiração do cache para esse repositório de armazenamento. O padrão é sem tempo limite, ou seja, quando um blob é lido, ele permanecerá no cache local até que o trabalho seja concluído. A maioria dos trabalhos preferirá essa configuração, mas alguns trabalhos precisarão ler mais dados de um grande DataSet do que caberá em seus nós. Para esses trabalhos, o ajuste desse parâmetro irá ajudá-lo a ter sucesso. Tome cuidado ao ajustar esse parâmetro: definir o valor muito baixo pode resultar em baixo desempenho, pois os dados usados em uma época podem expirar antes de serem usados novamente. Isso significa que todas as leituras serão feitas do armazenamento de BLOBs (ou seja, a rede) em vez do cache local, o que afeta negativamente os tempos de treinamento.
    + A descrição do modelo agora pode ser atualizada corretamente após o registro
    + A exclusão de modelo e imagem agora fornece mais informações sobre objetos upstream que dependem deles, o que causa a falha da exclusão
    + Melhorar a utilização de recursos de execuções remotas usando o azureml. mlflow.
  + **azureml-explain-model**
    + Argumento de transformações fixas para o explicador de verde-limão para importância de recurso bruto no azureml-contrib-explique-Package
    + Adicionar suporte esparso scipy para LimeExplainer
    + wrapper de explicador linear de forma adicionada, bem como outro nível para explicador tabular para explicar modelos lineares
    + para o explicador de imitação em explicar a biblioteca de modelos, erro fixo quando include_local = false para entrada de dados esparsas
    + Adicionar valores esperados `automl` à saída
    + correção do recurso de permutação fixa quando o argumento de transformações foi fornecido para obter a importância do recurso bruto
    + Adicionado `batch_size` ao explicador de imitação quando `include_local=False`, para transmitir explicações globais em lotes para melhorar o tempo de execução de DecisionTreeExplainableModel
    + para a biblioteca de explicabilidade de modelo, os explicadores Blackbox fixos em que a entrada do dataframe do pandas é necessária para previsão
    + Correção de um bug `explanation.expected_values` em que às vezes retornaria um float em vez de uma lista com um float.
  + **azureml-mlflow**
    + Melhorar o desempenho de mlflow. set_experiment (experiment_name)
    + Corrigir o bug em uso de InteractiveLoginAuthentication para mlflow tracking_uri
    + Melhorar a utilização de recursos de execuções remotas usando o azureml. mlflow.
    + Melhorar a documentação do pacote azureml-mlflow
    + Bug de patch em que mlflow. log_artifacts ("my_dir") salvaria artefatos em "my_dir/<artefato-Paths>" em vez de "<artefato-Paths>"
  + **azureml-opendatasets**
    + PIN `pyarrow` de `opendatasets` para versões antigas (<0.14.0) devido a um problema de memória introduzido recentemente.
    + Mova o azureml-contrib-opendatasets para o azureml-opendatasets.
    + Permita que as classes Open DataSet sejam registradas no espaço de trabalho Azure Machine Learning e aproveitem as funcionalidades do conjunto de recursos AML diretamente.
    + Melhore significativamente o desempenho do NoaaIsdWeather enriquecer na versão que não seja do SPARK.
  + **azureml-pipeline-steps**
    + O DBFS datastore agora tem suporte para entradas e saídas em DatabricksStep.
    + Documentação atualizada para a etapa do lote do Azure com relação a entradas/saídas.
    + Em AzureBatchStep, alterado *delete_batch_job_after_finish* valor padrão para *true*.
  + **azureml – telemetria**
    +  Mova o azureml-contrib-opendatasets para o azureml-opendatasets.
    + Permita que as classes Open DataSet sejam registradas no espaço de trabalho Azure Machine Learning e aproveitem as funcionalidades do conjunto de recursos AML diretamente.
    + Melhore significativamente o desempenho do NoaaIsdWeather enriquecer na versão que não seja do SPARK.
  + **azureml-Train-automl**
    + Documentação atualizada em get_output para refletir o tipo de retorno real e fornecer observações adicionais sobre a recuperação de propriedades de chave.
    + Atualize a dependência NimbusML para a versão 1.2.0 (atual mais recente).
    + Adicionar valores esperados `automl` à saída
  + **azureml-train-core**
    + As cadeias de caracteres agora são aceitas como destino de computação para ajuste de hiperparâmetro automatizado
    + A configuração RunConfiguration não utilizada auto_prepare_environment foi marcada como preterida.

### <a name="azure-machine-learning-data-prep-sdk-v119"></a>SDK v 1.1.9 do Azure Machine Learning data Prep

+ **Novos recursos**
  + Adicionado suporte para ler um arquivo diretamente de uma URL http ou HTTPS.

+ **Correções de bugs e melhorias**
  + Mensagem de erro aprimorada ao tentar ler um conjunto de parquet de uma fonte remota (que não tem suporte no momento).
  + Corrigido um bug ao gravar no formato de arquivo parquet no ADLS Gen 2 e atualizar o nome do contêiner ADLS Gen 2 no caminho.

## <a name="2019-07-09"></a>2019-07-09

### <a name="visual-interface"></a>Interface Visual
+ **Recursos de visualização**
  + Módulo "executar script R" adicionado na interface visual.

### <a name="azure-machine-learning-sdk-for-python-v1048"></a>Azure Machine Learning SDK para Python v 1.0.48

+ **Novos recursos**
  + **azureml-opendatasets**
    + o **azureml-contrib-opendatasets** agora está disponível como **azureml-opendatasets**. O pacote antigo ainda pode funcionar, mas é recomendável usar o **azureml-opendatasets** avançando para obter recursos e aprimoramentos mais avançados.
    + Esse novo pacote permite que você registre conjuntos de registros abertos como DataSet no espaço de trabalho Azure Machine Learning e aproveite quaisquer funcionalidades oferecidas pelo conjunto de recursos.
    + Ele também inclui recursos existentes, como o consumo de conjuntos de valores abertos como o pandas/SPARK dataframes e junções de local para alguns conjuntos de um conjunto de recursos, como clima.

+ **Recursos de visualização**
    + HyperDriveConfig agora pode aceitar o objeto de pipeline como um parâmetro para dar suporte ao ajuste de hiperparâmetro usando um pipeline.

+ **Correções de bugs e melhorias**
  + **azureml-Train-automl**
    + Corrigido o bug sobre a perda de tipos de colunas após a transformação.
    + Foi corrigido o bug para permitir que y_query seja um tipo de objeto que contenha nenhum (s) no início.
    + Corrigido o problema no procedimento de seleção de Ensemble que estava aumentando desnecessariamente o Ensemble resultante, mesmo se as pontuações permaneceram constantes.
    + Corrigido o problema com as configurações whitelist_models e blacklist_models em AutoMLStep.
    + Correção do problema que impedia o uso de pré-processamento quando AutoML teria sido usado no contexto de pipelines do Azure ML.
  + **azureml-opendatasets**
    + Você moveu o azureml-contrib-opendatasets para o azureml-opendatasets.
    + As classes Open DataSet permitidas foram registradas para Azure Machine Learning espaço de trabalho e aproveitam as funcionalidades do conjunto de recursos AML diretamente.
    + Desempenho aprimorado do NoaaIsdWeather enriquecer com uma versão não SPARK significativamente.
  + **azureml-explain-model**
    + Documentação online atualizada para objetos de interpretação.
    + Adicionado `batch_size` ao explicador de imitação quando `include_local=False`, para transmitir explicações globais em lotes para melhorar o tempo de execução de DecisionTreeExplainableModel para a biblioteca de explicabilidade de modelo.
    + Correção do problema em `explanation.expected_values` que às vezes retornaria um float em vez de uma lista com um float.
    + Adicionados valores esperados `automl` para saída para o explicador de imitação na biblioteca de modelos de explicação.
    + Correção do recurso de permutação corrigida quando o argumento de transformações foi fornecido para obter a importância do recurso bruto.
  + **azureml-núcleo**
    + Adicionada a capacidade de anexar armazenamentos de DBFS na CLI do AzureML.
    + Corrigido o problema com o carregamento do repositório de armazenamento em que uma pasta `target_path` vazia é `/`criada, se iniciada com.
    + Comparação habilitada de dois conjuntos de valores.
    + O modelo e a exclusão de imagem agora fornecem mais informações sobre como recuperar objetos upstream que dependem deles se a exclusão falhar devido a uma dependência upstream.
    + Preterida a configuração RunConfiguration não utilizada no auto_prepare_environment.
  + **azureml-mlflow**
    + Melhor utilização de recursos de execuções remotas que usam o azureml. mlflow.
    + A documentação do pacote azureml-mlflow foi aprimorada.
    + Correção do problema em que mlflow. log_artifacts ("my_dir") salvaria artefatos em "my_dir/Artifact-Paths" em vez de "artefatos".
  + **azureml-pipeline-Core**
    + O parâmetro hash_paths para todas as etapas de pipeline foi preterido e será removido no futuro. Por padrão, o conteúdo do source_directory é com hash (exceto os arquivos listados em. amlignore ou. gitignore)
    + Melhorando o módulo e o ModuleStep para dar suporte a módulos de tipo de computação específicos, para se preparar para a integração do RunConfiguration e outras alterações para desbloquear o uso do módulo específico do tipo de computação em pipelines.
  + **azureml-pipeline-steps**
    + AzureBatchStep: documentação aprimorada com relação a entradas/saídas.
    + AzureBatchStep: alterou delete_batch_job_after_finish valor padrão para true.
  + **azureml-train-core**
    + As cadeias de caracteres agora são aceitas como destino de computação para ajuste de hiperparâmetro automatizado.
    + Preterida a configuração RunConfiguration não utilizada no auto_prepare_environment.
    + Parâmetros `conda_dependencies_file_path` preteridos `pip_requirements_file_path` e em favor `conda_dependencies_file` de `pip_requirements_file` e, respectivamente.
  + **azureml-opendatasets**
    + Melhore significativamente o desempenho do NoaaIsdWeather enriquecer na versão que não seja do SPARK.

### <a name="azure-machine-learning-data-prep-sdk-v118"></a>SDK v 1.1.8 do Azure Machine Learning data Prep

+ **Novos recursos**
 + Os objetos Dataflow agora podem ser iterados, produzindo uma sequência de registros. Consulte a documentação `Dataflow.to_record_iterator`do.
  + Os objetos Dataflow agora podem ser iterados, produzindo uma sequência de registros. Consulte a documentação `Dataflow.to_record_iterator`do.

+ **Correções de bugs e melhorias**
 + Aumentou a robustez do dataprep SDK.
 + Tratamento aprimorado de quadros de bits pandas com índices de coluna que não são de cadeia de caracteres.
 + Melhorou o desempenho `to_pandas_dataframe` do em conjuntos de os.
 + Correção de um bug em que a execução do Spark de conjuntos de valores falhou quando executada em um ambiente com vários nós.
  + Aumentou a robustez do dataprep SDK.
  + Tratamento aprimorado de quadros de bits pandas com índices de coluna que não são de cadeia de caracteres.
  + Melhorou o desempenho `to_pandas_dataframe` do em conjuntos de os.
  + Correção de um bug em que a execução do Spark de conjuntos de valores falhou quando executada em um ambiente com vários nós.

## <a name="2019-07-01"></a>2019-07-01

### <a name="azure-machine-learning-data-prep-sdk-v117"></a>SDK v 1.1.7 do Azure Machine Learning data Prep

Revertemos uma alteração que melhorou o desempenho, pois ela estava causando problemas para alguns clientes usando Azure Databricks. Se você recebeu um problema em Azure Databricks, você pode atualizar para a versão 1.1.7 usando um dos métodos abaixo:
1. Execute este script para atualizar:`%sh /home/ubuntu/databricks/python/bin/pip install azureml-dataprep==1.1.7`
2. Recrie o cluster, que instalará a versão mais recente do SDK de preparação de dados.

## <a name="2019-06-25"></a>2019-06-25

### <a name="azure-machine-learning-sdk-for-python-v1045"></a>Azure Machine Learning SDK para Python v 1.0.45

+ **Novos recursos**
  + Adicionar modelo substituto de árvore de decisão ao explicador imitador em azureml-explique-pacote de modelo
  + Capacidade de especificar uma versão do CUDA a ser instalada em imagens do inferência. Suporte para CUDA 9,0, 9,1 e 10,0.
  + As informações sobre as imagens de base de treinamento do Azure ML agora estão disponíveis no [repositório do GitHub de contêineres do Azure ml](https://github.com/Azure/AzureML-Containers) e [DockerHub](https://hub.docker.com/_/microsoft-azureml)
  + Adicionado suporte à CLI para o agendamento do pipeline. Execute "AZ ml pipeline-h" para saber mais
  + Adicionado o parâmetro namespace kubernetes personalizado à CLI e à configuração de implantação do AKS WebService.
  + Parâmetro de hash_paths preterido para todas as etapas de pipeline
  + Model. Register agora dá suporte ao registro de vários arquivos individuais como um único modelo com o `child_paths` uso do parâmetro.

+ **Recursos de visualização**
    + Os explicadores de Pontuação agora podem, opcionalmente, salvar informações de Conda e Pip para serialização e desserialização mais confiáveis.
    + Correção de bug do seletor de recurso automático.
    + Atualizado mlflow. azureml. build_image para a nova API, bugs com patch expostos pela nova implementação.

+ **Correções de bugs e melhorias**
  + Dependência `paramiko` removida do azureml-Core. Foram adicionados avisos de substituição para métodos de anexação de destino de computação herdados.
  + Melhorar o desempenho da execução. create_children
  + Em explicador de imitação com classificador binário, corrija a ordem das probabilidades quando a probabilidade de professor for usada para dimensionar valores de forma.
  + Tratamento de erros e mensagem aprimorados para o aprendizado de máquina automatizado.
  + Correção do problema de tempo limite de iteração para o aprendizado de máquina automatizado.
  + Melhorou o desempenho de transformação da série temporal para o aprendizado de máquina automatizado.

## <a name="2019-06-24"></a>2019-06-24

### <a name="azure-machine-learning-data-prep-sdk-v116"></a>SDK v 1.1.6 do Azure Machine Learning data Prep

+ **Novos recursos**
  + Funções de resumo adicionadas para valores`SummaryFunction.TOPVALUES`principais () e valores`SummaryFunction.BOTTOMVALUES`inferiores ().

+ **Correções de bugs e melhorias**
  + Melhorou significativamente o desempenho `read_pandas_dataframe`do.
  + Foi corrigido um bug que causava `get_profile()` uma falha em um fluxo de arquivos que apontasse para arquivo binário.
  + Exposto `set_diagnostics_collection()` para permitir a habilitação/desabilitação programática da coleção de telemetria.
  + Alterou o comportamento `get_profile()`de. Os valores NaN agora são ignorados por min, mean, STD e Sum, que se alinha com o comportamento de pandas.


## <a name="2019-06-10"></a>2019-06-10

### <a name="azure-machine-learning-sdk-for-python-v1043"></a>Azure Machine Learning SDK para Python v 1.0.43

+ **Novos recursos**
  + O Azure Machine Learning agora fornece suporte de primeira classe para aprendizado de máquina e estrutura de análise de dados populares Scikit. Usando [ `SKLearn` o estimador](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py), os usuários podem facilmente treinar e implantar modelos Scikit-learn.
    + Saiba como [executar o ajuste de hiperparâmetro com Scikit-Learn usando o hyperdrive](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb).
  + Adição de suporte para a criação de ModuleStep em pipelines junto com classes de módulo e ModuleVersion para gerenciar unidades de computação reutilizáveis.
  + Os WebServices ACI agora dão suporte a scoring_uri persistentes por meio de atualizações. O scoring_uri será alterado de IP para FQDN. O rótulo de nome DNS para FQDN pode ser configurado definindo o dns_name_label em deploy_configuration.
  + Novos recursos do Machine Learning automatizados:
    + Featurizer STL para previsão
    + O clustering do KMeans está habilitado para a limpeza de recursos
  + As aprovações de cota AmlCompute se tornaram mais rápidas! Agora, automatizamos o processo para aprovar suas solicitações de cota dentro de um limite. Para obter mais informações sobre como as cotas funcionam, saiba [como gerenciar cotas](https://docs.microsoft.com/azure/machine-learning/how-to-manage-quotas).

+ **Recursos de visualização**
    + Integração com o [MLflow](https://mlflow.org) 1.0.0 Tracking por meio do pacote azureml-MLflow ([notebooks de exemplo](https://aka.ms/azureml-mlflow-examples)).
    + Envie o bloco de anotações Jupyter como uma execução. [Documentação de referência de API](https://docs.microsoft.com/python/api/azureml-contrib-notebook/azureml.contrib.notebook?view=azure-ml-py)
    + Visualização pública do [detector de descompasso de dados](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector(class)) por meio do pacote azureml-contrib-descompasso ([notebooks de exemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/monitor-models/data-drift)). A descompasso de dados é uma das principais razões em que a precisão do modelo diminui com o passar do tempo. Acontece quando os dados servidos para o modelo na produção são diferentes dos dados em que o modelo foi treinado. O detector de descompasso de dados AML ajuda o cliente a monitorar descompasso de dados e envia alerta sempre que o descompasso é detectado.

+ **Alterações de quebra**

+ **Correções de bugs e melhorias**
  + O RunConfiguration Load e Save dá suporte à especificação de um caminho de arquivo completo com compatibilidade total para o comportamento anterior.
  + Cache adicionado em ServicePrincipalAuthentication, desativado por padrão.
  + Habilite o registro em log de várias plotagens com o mesmo nome de métrica.
  + A classe de modelo agora é adequadamente importável de azureml`from azureml.core import Model`. Core ().
  + Nas etapas do pipeline `hash_path` , o parâmetro agora é preterido. O novo comportamento é o hash completo source_directory, exceto os arquivos listados em. amlignore ou. gitignore.
  + Em pacotes de pipeline, `get_all` vários `get_all_*` métodos e foram preteridos em favor `list` de `list_*`e, respectivamente.
  + o azureml. Core. get_run não requer mais classes a serem importadas antes de retornar o tipo de execução original.
  + Corrigido um problema em que algumas chamadas para a atualização do WebService não dispararam uma atualização.
  + O tempo limite de pontuação em WebServices AKS deve estar entre 5 ms e 300000ms. O scoring_timeout_ms máximo permitido para solicitações de pontuação foi relevo de 1 min a 5 min.
  + Os objetos LocalWebservice agora `scoring_uri` têm `swagger_uri` Propriedades e.
  + A criação do diretório de saídas movidos e o carregamento do diretório de saídas do processo do usuário. O SDK do histórico de execução habilitado deve ser executado em cada processo do usuário. Isso deve resolver alguns problemas de sincronização experientes por execuções de treinamento distribuídas.
  + O nome do log do azureml gravado no nome do processo do usuário agora incluirá o nome do processo (somente para treinamento distribuído) e PID.

### <a name="azure-machine-learning-data-prep-sdk-v115"></a>SDK v 1.1.5 do Azure Machine Learning data Prep

+ **Correções de bugs e melhorias**
  + Para valores de data e hora interpretados que têm um formato de ano de 2 dígitos, o intervalo de anos válidos foi atualizado para corresponder ao Windows pode ser liberado. O intervalo foi alterado de 1930-2029 para 1950-2049.
  + Ao ler um arquivo e configurar `handleQuotedLineBreaks=True`, `\r` será tratado como uma nova linha.
  + Corrigido um bug que causou `read_pandas_dataframe` a falha em alguns casos.
  + Desempenho aprimorado `get_profile`do.
  + Mensagens de erro aprimoradas.

## <a name="2019-05-28"></a>2019-05-28

### <a name="azure-machine-learning-data-prep-sdk-v114"></a>SDK v 1.1.4 do Azure Machine Learning data Prep

+ **Novos recursos**
  + Agora você pode usar as seguintes funções de linguagem de expressão para extrair e analisar valores DateTime em novas colunas.
    + `RegEx.extract_record()`extrai elementos DateTime em uma nova coluna.
    + `create_datetime()`cria objetos DateTime a partir de elementos DateTime separados.
  + Ao chamar `get_profile()`, agora você pode ver que as colunas Quantil são rotuladas como (est.) para indicar claramente que os valores são aproximações.
  + Agora você pode usar o * * mascaramento ao ler do armazenamento de BLOBs do Azure.
    + Por exemplo: `dprep.read_csv(path='https://yourblob.blob.core.windows.net/yourcontainer/**/data/*.csv')`

+ **Correções de bugs**
  + Correção de um bug relacionado à leitura de um arquivo parquet de uma origem remota (blob do Azure).

## <a name="2019-05-14"></a>2019-05-14

### <a name="azure-machine-learning-sdk-for-python-v1039"></a>Azure Machine Learning SDK para Python v 1.0.39
+ **Alterações**
  + A opção de auto_prepare_environment de configuração de execução está sendo preterida, com a preparação automática se tornando o padrão.

## <a name="2019-05-08"></a>2019-05-08

### <a name="azure-machine-learning-data-prep-sdk-v113"></a>SDK v 1.1.3 do Azure Machine Learning data Prep

+ **Novos recursos**
  + Adicionado suporte para leitura de um banco de dados PostgresSQL, seja chamando read_postgresql ou usando um repositório de armazenamento.
    + Consulte os exemplos nos guias de instruções:
      + [Bloco de anotações de ingestão de dados](https://aka.ms/aml-data-prep-ingestion-nb)
      + [Bloco de anotações do repositório de armazenamento](https://aka.ms/aml-data-prep-datastore-nb)

+ **Correções de bugs e melhorias**
  + Problemas corrigidos com a conversão de tipo de coluna:
  + Agora, converte corretamente uma coluna Booleana ou numérica em uma coluna Booleana.
  + Agora não falha ao tentar definir uma coluna de data como tipo de data.
  + Os tipos de JoinType aprimorados e a documentação de referência acompanhada. Ao unir dois fluxos de entrada, agora você pode especificar um destes tipos de junção:
    + NENHUM, CORRESPONDER, INTERNO, UNMATCHLEFT, LEFTANTI, LEFTOUTER, UNMATCHRIGHT, RIGHTANTI, RIGHTOUTER, FULLANTI, FULL.
  + Tipo de dados aprimorado inferência para reconhecer mais formatos de data.

## <a name="2019-05-06"></a>2019-05-06

### <a name="azure-portal"></a>Portal do Azure

No portal do Azure, agora você pode:
+ Criar e executar experimentos de ML automatizados
+ Crie uma VM do notebook para experimentar os notebooks Jupyter de exemplo ou seus próprios.
+ Seção de criação de nova marca (versão prévia) no espaço de trabalho Azure Machine Learning, que inclui Machine Learning automatizado, interface visual e VMs de notebook hospedadas
    + Criar automaticamente um modelo usando o Machine Learning automatizado
    + Usar uma interface visual de arrastar e soltar para executar experimentos
    + Crie uma VM de notebook para explorar dados, criar modelos e implantar serviços.
+ Gráfico dinâmico e atualização de métrica em executar relatórios e páginas de detalhes de execução
+ Atualizado o Visualizador de arquivos para logs, saídas e instantâneos em páginas de detalhes de execução.
+ Nova e aprimorada experiência de criação de relatórios na guia experimentos.
+ Foi adicionada a capacidade de baixar o arquivo config. JSON da página Visão geral do espaço de trabalho Azure Machine Learning.
+ Suporte à criação de Azure Machine Learning espaço de trabalho no espaço de trabalho Azure Databricks.

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033"></a>Azure Machine Learning SDK para Python v 1.0.33
+ **Novos recursos**
  + O método _Workspace. Create_ agora aceita configurações de cluster padrão para clusters de CPU e GPU.
  + Se a criação do espaço de trabalho falhar, os recursos dependentes serão limpos.
  + O SKU do registro de contêiner do Azure padrão foi alternado para básico.
  + O registro de contêiner do Azure é criado lentamente, quando necessário para a execução ou criação de imagem.
  + Suporte para ambientes para execuções de treinamento.

### <a name="notebook-virtual-machine"></a>Máquina virtual do notebook 

Use uma VM de notebook como um ambiente de hospedagem seguro e pronto para a empresa para notebooks Jupyter nos quais você pode programar experimentos de aprendizado de máquina, implantar modelos como pontos de extremidade da Web e executar todas as outras operações com suporte pelo SDK Azure Machine Learning usando Python.Ele fornece vários recursos:
+ [Crie rapidamente uma VM](tutorial-1st-experiment-sdk-setup.md) de notebook pré-configurada que tenha a versão mais recente do SDK do Azure Machine Learning e os pacotes relacionados.
+ O acesso é protegido por meio de tecnologias comprovadas, como HTTPS, autenticação e autorização de Azure Active Directory.
+ Armazenamento em nuvem confiável de blocos de anotações e código em sua conta de armazenamento de BLOBs Workspace do Azure Machine Learning.Você pode excluir com segurança sua VM do bloco de anotações sem perder seu trabalho.
+ Blocos de anotações de exemplo pré-instalados para explorar e experimentar Azure Machine Learning recursos.
+ Recursos de personalização completa de VMs do Azure, qualquer tipo de VM, qualquer pacote, qualquer driver. 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033-released"></a>Azure Machine Learning SDK para Python v 1.0.33 lançado.

+ O Modelos de Aceleração de Hardware do Azure ML no [FPGAs](how-to-deploy-fpga-web-service.md) está disponível para o público em geral.
  + Agora você pode [usar o pacote azureml-da aceleração extra-Models](how-to-deploy-fpga-web-service.md) para:
    + Treinar os pesos de uma rede neural profunda com suporte (ResNet 50, ResNet 152, DenseNet-121, VGG-16 e SSD-VGG)
    + Usar o aprendizado de transferência com o DNN com suporte
    + Registrar o modelo com o Serviço Gerenciamento de Modelos e colocar o modelo em contêiner
    + Implantar o modelo em uma VM do Azure com um FPGA em um cluster do AKS (serviço kubernetes do Azure)
  + Implantar o contêiner em um dispositivo de servidor [Azure data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview)
  + Pontuar seus dados com o ponto de extremidade gRPC com este [exemplo](https://github.com/Azure-Samples/aml-hardware-accelerated-models)

### <a name="automated-machine-learning"></a>Automatizado de Machine Learning

+ Limpeza de recursos para permitir a adição :::no-loc text="featurizers"::: dinâmica para otimização de desempenho. Novo :::no-loc text="featurizers":::: incorporações de trabalho, peso de evidência, codificações de destino, codificação de destino de texto, distância do cluster
+ Smart currículo para lidar com as divisões de treinamento/válidas dentro do ML automatizado
+ Poucas alterações de otimização de memória e melhoria no desempenho de tempo de execução
+ Melhoria de desempenho na explicação do modelo
+ Conversão de modelo ONNX para execução local
+ Suporte adicionado à subamostragem
+ Parando inteligente quando não há critérios de saída definidos
+ Conjuntos empilhados

+ Previsão de série temporal
  + Nova função prever previsão
  + Agora você pode usar a validação cruzada de origem sem interrupção em dados de série temporal
  + Nova funcionalidade adicionada para configurar o tempo de retardo da série temporal
  + Nova funcionalidade adicionada para dar suporte a recursos agregados de janela sem interrupção
  + Nova detecção de feriados e featurizer quando o código do país é definido nas configurações do experimento

+ Azure Databricks
  + Funcionalidade de previsão de série temporal e modelo de explainabilty/interpretação habilitada
  + Agora você pode cancelar e retomar (continuar) experimentos de ML automatizados
  + Suporte adicionado para processamento de vários núcleos

### <a name="mlops"></a>MLOps
+ **Implantação local & depuração para contêineres de Pontuação**<br/> Agora você pode implantar um modelo de ML localmente e iterar rapidamente em seu arquivo de Pontuação e dependências para garantir que eles se comportem conforme o esperado.

+ **Introduziu o modelo de & InferenceConfig. Deploy ()**<br/> A implantação de modelo agora dá suporte à especificação de uma pasta de origem com um script de entrada, o mesmo que um RunConfig.  Além disso, a implantação do modelo foi simplificada para um único comando.

+ **Acompanhamento de referência do git**<br/> Os clientes estão solicitando recursos básicos de integração do git por algum tempo, pois ele ajuda a manter uma trilha de auditoria de ponta a ponta. Implementamos o rastreamento em entidades principais no Azure ML para metadados relacionados ao git (repositório, confirmação, estado de limpeza). Essas informações serão coletadas automaticamente pelo SDK e pela CLI.

+ **Criação de perfil do modelo & serviço de validação**<br/> Os clientes freqüentemente reclamam da dificuldade de dimensionar corretamente a computação associada ao seu serviço de inferência. Com nosso serviço de criação de perfil de modelo, o cliente pode fornecer entradas de exemplo e criarei um perfil em 16 diferentes configurações de CPU/memória para determinar o dimensionamento ideal para a implantação.

+ **Traga sua própria imagem base para inferência**<br/> Outra reclamação comum foi a dificuldade de migrar da experimentação para a inferência recompartilhamento de dependências. Com nosso novo recurso de compartilhamento de imagem base, agora você pode reutilizar suas imagens base de experimentação, dependências e todas, para inferência. Isso deve acelerar as implantações e reduzir a lacuna do loop interno para o exterior.

+ **Experiência de geração de esquema do Swagger aprimorada**<br/> Nosso método de geração Swagger anterior era propenso a erros e impossível de ser automatizado. Temos uma nova maneira embutida de gerar esquemas do Swagger de qualquer função do Python por meio de decoradores. Nós abrimos esse código e nosso protocolo de geração de esquema não está acoplado à plataforma ML do Azure.

+ **O CLI do Azure ML está geralmente disponível (GA)**<br/> Os modelos agora podem ser implantados com um único comando da CLI. Temos comentários comuns do cliente que ninguém implanta um modelo de ML de um notebook Jupyter. A [**documentação de referência da CLI**](https://aka.ms/azmlcli) foi atualizada.


## <a name="2019-04-22"></a>2019-04-22

Azure Machine Learning SDK para Python v 1.0.30 lançado.

O [`PipelineEndpoint`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint?view=azure-ml-py) foi introduzido para adicionar uma nova versão de um pipeline publicado enquanto mantém o mesmo ponto de extremidade.

## <a name="2019-04-17"></a>2019-04-17

### <a name="azure-machine-learning-data-prep-sdk-v112"></a>Azure Machine Learning o SDK da preparação de dados v 1.1.2

Observação: o SDK do Python de preparação de dados `numpy` não `pandas` será mais instalado e empacotado. Consulte [as instruções de instalação atualizadas](https://github.com/Microsoft/AMLDataPrepDocs).

+ **Novos recursos**
  + Agora você pode usar a transformação dinâmica.
    + Guia de instruções: [bloco de anotações dinâmico](https://aka.ms/aml-data-prep-pivot-nb)
  + Agora você pode usar expressões regulares em funções nativas.
    + Exemplos:
      + `dflow.filter(dprep.RegEx('pattern').is_match(dflow['column_name']))`
      + `dflow.assert_value('column_name', dprep.RegEx('pattern').is_match(dprep.value))`
  + Agora você pode usar `to_upper`  o `to_lower`  e o Functions na linguagem de expressão.
  + Agora você pode ver o número de valores exclusivos de cada coluna em um perfil de dados.
  + Para algumas das etapas mais usadas do leitor, agora você pode passar o `infer_column_types` argumento. Se estiver definido como, `True`a preparação de dados tentará detectar e converter automaticamente os tipos de coluna.
    + `inference_arguments`Agora está preterido.
  + Agora você pode chamar `Dataflow.shape`.

+ **Correções de bugs e melhorias**
  + `keep_columns` Agora aceita um argumento `validate_column_exists`opcional adicional, que verifica se o resultado de `keep_columns` conterá qualquer coluna.
  + Todas as etapas do leitor (que lêem de um arquivo) agora aceitam um `verify_exists`argumento opcional adicional.
  + Desempenho aprimorado de leitura do data frame do pandas e obtenção de perfis de dados.
  + Correção de um bug em que houve falha na divisão de uma única etapa de um Dataflow com um único índice.

## <a name="2019-04-15"></a>2019-04-15

### <a name="azure-portal"></a>Portal do Azure
  + Agora você pode reenviar um script existente executado em um cluster de computação remota existente.
  + Agora você pode executar um pipeline publicado com novos parâmetros na guia pipelines.
  + Os detalhes da execução agora dão suporte a um novo Visualizador de arquivos de instantâneo. Você pode exibir um instantâneo do diretório ao enviar uma execução específica. Você também pode baixar o bloco de anotações que foi enviado para iniciar a execução.
  + Agora você pode cancelar as execuções pai do portal do Azure.

## <a name="2019-04-08"></a>2019-04-08

### <a name="azure-machine-learning-sdk-for-python-v1023"></a>Azure Machine Learning SDK para Python v 1.0.23

+ **Novos recursos**
  + O SDK do Azure Machine Learning agora dá suporte ao Python 3,7.
  + Azure Machine Learning os estimadores DNN agora fornecem suporte interno a várias versões. Por exemplo, `TensorFlow`  o estimador agora `framework_version` aceita um parâmetro e os usuários podem especificar a versão ' 1,10 ' ou ' 1,12 '. Para obter uma lista das versões com suporte na versão atual do SDK, `get_supported_versions()` chame na classe da estrutura desejada (por exemplo `TensorFlow.get_supported_versions()`,).
  Para obter uma lista das versões com suporte da versão mais recente do SDK, consulte a [documentação do estimador do DNN](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py).

### <a name="azure-machine-learning-data-prep-sdk-v111"></a>SDK v 1.1.1 do Azure Machine Learning data Prep

+ **Novos recursos**
  + Você pode ler várias fontes de datastore/caminho de email/datareferente usando transformações read_ *.
  + Você pode executar as seguintes operações em colunas para criar uma nova coluna: divisão, piso, módulo, potência, comprimento.
  + A preparação de dados agora faz parte do pacote de diagnóstico do Azure ML e registrará em log as informações de diagnóstico por padrão.
    + Para desativar essa opção, defina essa variável de ambiente como true: DISABLE_DPREP_LOGGER

+ **Correções de bugs e melhorias**
  + Documentação de código aprimorada para classes e funções usadas com frequência.
  + Correção de um bug no auto_read_file que não leu arquivos do Excel.
  + Opção adicionada para substituir a pasta em read_pandas_dataframe.
  + Desempenho aprimorado da instalação de dependência do dotnetcore2 e adição de suporte para Fedora 27/28 e Ubuntu 1804.
  + Melhorou o desempenho da leitura de BLOBs do Azure.
  + A detecção de tipo de coluna agora dá suporte a colunas do tipo Long.
  + Correção de um bug em que alguns valores de data estavam sendo exibidos como carimbos de hora em vez de objetos DateTime de Python.
  + Corrigido um bug em que algumas contagens de tipo estavam sendo exibidas como duplos em vez de inteiros.


## <a name="2019-03-25"></a>25-03-2019

### <a name="azure-machine-learning-sdk-for-python-v1021"></a>Azure Machine Learning SDK para Python v 1.0.21

+ **Novos recursos**
  + O método *azureml. Core. Run. create_children* permite a criação de baixa latência de várias execuções filho com uma única chamada.

### <a name="azure-machine-learning-data-prep-sdk-v110"></a>SDK da preparação de dados do Azure Machine Learning v 1.1.0

+ **Alterações de quebra**
  + O conceito do pacote de preparação de dados foi preterido e não tem mais suporte. Em vez de persistir vários fluxos de entrada em um pacote, você pode persistir os fluxos de entrada individualmente.
    + Guia de instruções: [abrindo e salvando o notebook de fluxos de anotações](https://aka.ms/aml-data-prep-open-save-dataflows-nb)

+ **Novos recursos**
  + A preparação de dados agora pode reconhecer colunas que correspondem a um tipo semântico específico e divididas de acordo. Os STypes atualmente suportados incluem: endereço de email, coordenadas geográficas (Latitude & longitude), endereços IPv4 e IPv6, número de telefone dos EUA e CEP dos EUA.
    + Guia de instruções: [bloco de anotações de tipos semânticos](https://aka.ms/aml-data-prep-semantic-types-nb)
  + A preparação de dados agora dá suporte às seguintes operações para gerar uma coluna resultante de duas colunas numéricas: subtrair, multiplicar, dividir e módulo.
  + Você pode chamar `verify_has_data()` em um Dataflow para verificar se o Dataflow produziria registros, se executado.

+ **Correções de bugs e melhorias**
  + Agora você pode especificar o número de compartimentos a serem usados em um histograma para perfis de coluna numérica.
  + Agora `read_pandas_dataframe` , a transformação requer que o dataframe tenha nomes de coluna com tipo de cadeia de caracteres ou de byte.
  + Corrigido um bug na `fill_nulls` transformação, em que os valores não foram preenchidos corretamente se a coluna estava ausente.

## <a name="2019-03-11"></a>2019-03-11

### <a name="azure-machine-learning-sdk-for-python-v1018"></a>Azure Machine Learning SDK para Python v 1.0.18

 + **Alterações**
   + O pacote azureml-tensorboard substitui o azureml-contrib-tensorboard.
   + Com esta versão, você pode configurar uma conta de usuário em seu amlcompute (cluster de computação gerenciada), ao criá-la. Isso pode ser feito passando essas propriedades na configuração de provisionamento. Você pode encontrar mais detalhes na [documentação de referência do SDK](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--).

### <a name="azure-machine-learning-data-prep-sdk-v1017"></a>SDK v 1.0.17 do Azure Machine Learning data Prep

+ **Novos recursos**
  + Agora dá suporte à adição de duas colunas numéricas para gerar uma coluna resultante usando a linguagem de expressão.

+ **Correções de bugs e melhorias**
  + Melhorou a documentação e a verificação de parâmetros para random_split.

## <a name="2019-02-27"></a>2019-02-27

### <a name="azure-machine-learning-data-prep-sdk-v1016"></a>SDK v 1.0.16 do Azure Machine Learning data Prep

+ **Correção de bug**
  + Correção de um problema de autenticação de entidade de serviço que foi causado por uma alteração de API.

## <a name="2019-02-25"></a>2019-02-25

### <a name="azure-machine-learning-sdk-for-python-v1017"></a>Azure Machine Learning SDK para Python v 1.0.17

+ **Novos recursos**

  + Azure Machine Learning agora fornece suporte de primeira classe para o DNN Framework popular. Usar [`Chainer`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) usuários de classe pode facilmente treinar e implantar modelos de encadeamento.
    + Saiba como [executar o treinamento distribuído com o ChainerMN](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/distributed-chainer/distributed-chainer.ipynb)
    + Saiba como [executar o ajuste de hiperparâmetro com o encadeamento usando hyperdrive](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb)
  + Azure Machine Learning pipelines adicionou a capacidade de disparar uma execução de pipeline com base nas modificações do repositório de armazenamento. O [notebook de agendamento](https://aka.ms/pl-schedule) de pipeline é atualizado para demonstrar esse recurso.

+ **Correções de bugs e melhorias**
  + Adicionamos suporte em pipelines de Azure Machine Learning para definir a propriedade source_directory_data_store para um armazenamento de um repositório desejado (como um armazenamento de BLOBs) no [RunConfigurations](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) que são fornecidos para o [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py). Por padrão, as etapas usam o repositório de arquivos do Azure como o repositório de armazenamento de backup, que pode ter problemas de limitação quando um grande número de etapas é executado simultaneamente.

### <a name="azure-portal"></a>Portal do Azure

+ **Novos recursos**
  + Nova experiência de editor de tabela de arrastar e soltar para relatórios. Os usuários podem arrastar uma coluna do bem para a área de tabela em que uma visualização da tabela será exibida. As colunas podem ser reorganizadas.
  + Novo Visualizador de arquivos de logs
  + Links para execuções de teste, computação, modelos, imagens e implantações na guia atividades

### <a name="azure-machine-learning-data-prep-sdk-v1015"></a>SDK v 1.0.15 do Azure Machine Learning data Prep

+ **Novos recursos**
  + A preparação de dados agora dá suporte à gravação de fluxos de arquivo de um fluxo de arquivos. Também fornece a capacidade de manipular os nomes de fluxo de arquivo para criar novos nomes de arquivo.
    + Guia de instruções: [trabalhando com o bloco de anotações de fluxos de arquivos](https://aka.ms/aml-data-prep-file-stream-nb)

+ **Correções de bugs e melhorias**
  + Desempenho aprimorado de t-Digest em conjuntos de dados grandes.
  + A preparação de dados agora dá suporte à leitura de dados de um caminho de data.
  + Uma codificação ativa agora funciona em colunas booleanas e numéricas.
  + Outras correções de bugs diversas.

## <a name="2019-02-11"></a>2019-02-11

### <a name="azure-machine-learning-sdk-for-python-v1015"></a>SDK do Azure Machine Learning para Python v1.0.15

+ **Novos recursos**
  + Azure Machine Learning pipelines adicionou AzureBatchStep ([Notebook](https://aka.ms/pl-azbatch)), HyperDriveStep (Notebook) e a funcionalidade de agendamento baseada em tempo ([Notebook](https://aka.ms/pl-schedule)).
  +  DataTranferStep atualizado para funcionar com o Azure SQL Server e banco de dados do Azure para PostgreSQL ([notebook](https://aka.ms/pl-data-trans)).

+ **Alterações**
  + `PublishedPipeline.get_published_pipeline` preterido em favor de `PublishedPipeline.get`.
  + `Schedule.get_schedule` preterido em favor de `Schedule.get`.

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>SDK de Preparação de Dados do Azure Machine Learning v1.0.12

+ **Novos recursos**
  + A preparação de dados agora oferece suporte à leitura de um banco de dados SQL do Azure usando o armazenamento de dados.

+ **Alterações**
  + Melhorou o desempenho da memória de determinadas operações em dados grandes.
  + `read_pandas_dataframe()` agora exige que `temp_folder` seja especificado.
  + A propriedade `name` em `ColumnProfile` foi preterida - usar `column_name` em vez disso.

## <a name="2019-01-28"></a>28/01/2019

### <a name="azure-machine-learning-sdk-for-python-v1010"></a>SDK do Azure Machine Learning para Python v1.0.10

+ **Alterações**:
  + O SDK do Azure Machine Learning não tem mais pacotes azure-cli como dependência. Especificamente, as dependências azure-cli-core e azure-cli-profile foram removidas de azureml-core. Estas são alterações que afetam o usuário:
      + Se você estiver executando "AZ login" e usando o azureml-SDK, o SDK fará o navegador ou o log de código do dispositivo em mais uma vez. Ele não usará nenhum estado de credencial criado por "az login".
    + Para autenticação de CLI do Azure, assim como usando "az login", use a classe _azureml.core.authentication.AzureCliAuthentication_. Para a autenticação de CLI do Azure, execute _pip install azure-cli_ no ambiente do Python em que você instalou o azureml-sdk.
    + Se você estiver usando "az login" e usando uma entidade de serviço para a automação, é recomendável usar a classe _azureml.core.authentication.ServicePrincipalAuthentication_, pois o azureml-sdk não usará o estado de credenciais criado pela CLI do Azure.

+ **Correções de bugs**: esta versão contém principalmente correções de bugs secundárias

### <a name="azure-machine-learning-data-prep-sdk-v108"></a>SDK de Preparação de Dados do Azure Machine Learning v1.0.8

+ **Correções de bugs**
  + Melhorou o desempenho da obtenção de perfis de dados.
  + Bugs secundários relacionados ao relatório de erros foram corrigidos.

### <a name="azure-portal-new-features"></a>Portal do Azure: novos recursos
+ Nova experiência de criação de gráficos de arrastar e soltar para relatórios. Os usuários podem arrastar uma coluna ou um atributo do poço à área do gráfico, na qual o sistema selecionará automaticamente um tipo de gráfico apropriado para o usuário com base no tipo de dados. Os usuários podem alterar o tipo de gráfico para outros tipos aplicáveis ou adicionar outros atributos.

    Tipos de gráfico compatíveis:
    - Gráfico de linhas
    - Histograma
    - Gráfico de barras empilhadas
    - Gráfico de caixa
    - Dispersão
    - Gráfico de bolhas
+ O portal agora gera relatórios dinamicamente para experimentos. Quando um usuário envia uma execução a um experimento, um relatório será gerado automaticamente com métricas registradas em log e gráficos para permitir a comparação entre diferentes execuções.

## <a name="2019-01-14"></a>2019-01-14

### <a name="azure-machine-learning-sdk-for-python-v108"></a>SDK do Azure Machine Learning para Python v1.0.8

+ **Correções de bugs**: esta versão contém principalmente correções de bugs secundárias

### <a name="azure-machine-learning-data-prep-sdk-v107"></a>SDK de Preparação de Dados do Azure Machine Learning v1.0.7

+ **Novos recursos**
  + Aprimoramentos do repositório de dados (documentado em [Guia de instruções do repositório de dados](https://aka.ms/aml-data-prep-datastore-nb))
    + Capacidade adicional de ler e gravar no compartilhamento de Arquivo do Azure e em Repositórios de Dados do ADLS em expansão.
    + Ao usar repositórios de dados, a preparação de dados agora dá suporte ao uso à autenticação de entidade de serviço em vez da autenticação interativa.
    + Adicionado suporte para URLs de wasb e wasbs.

## <a name="2019-01-09"></a>2019-01-09

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>SDK de Preparação de Dados do Azure Machine Learning v1.0.6

+ **Correções de bugs**
  + Corrigido o bug com a leitura de contêineres de Blob do Azure legíveis públicos no Spark

## <a name="2018-12-20"></a>2018-12-20

### <a name="azure-machine-learning-sdk-for-python-v106"></a>SDK do Azure Machine Learning para Python v1.0.6
+ **Correções de bugs**: esta versão contém principalmente correções de bugs secundárias

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>SDK de preparação de dados do Azure Machine Learning v1.0.4

+ **Novos recursos**
  + A função `to_bool` agora permite que os valores incompatíveis sejam convertidos em valores de erro. Esse é o novo comportamento de incompatibilidade de padrão para `to_bool` e `set_column_types`, enquanto o comportamento padrão anterior era para converter valores incompatíveis como False.
  + Ao chamar `to_pandas_dataframe`, há uma nova opção para interpretar valores ausente/nulos em colunas numéricas como NaN.
  + Capacidade adicional para verificar o tipo de retorno de algumas expressões para garantir a consistência de tipo e falha antecipada.
  + Agora você pode chamar `parse_json` para analisar valores em uma coluna como objetos JSON e expandi-las em várias colunas.

+ **Correções de bugs**
  + Corrigido um bug que travou `set_column_types` no Python 3.5.2.
  + Corrigido um bug que falhou ao se conectar ao armazenamento de dados usando uma imagem do AML.

+ **Atualizações**
  * [Notebooks de exemplo](https://aka.ms/aml-data-prep-notebooks) para obter tutoriais de Introdução, estudos de caso e guias de instruções.

## <a name="2018-12-04-general-availability"></a>2018-12-04: disponibilidade geral

Azure Machine Learning agora está disponível para o público geral.

### <a name="azure-machine-learning-compute"></a>Computação do Azure Machine Learning
Com esta versão, estamos anunciando uma nova experiência de computação gerenciada por meio da [Computação do Machine Learning](how-to-set-up-training-targets.md#amlcompute). Esse destino de computação substitui a computação do IA do Lote do Azure para o Azure Machine Learning.

O destino de computação:
+ É usado para treinamento de modelo e inferência/Pontuação de lote
+ É único - para computação de vários nós
+ Realiza o gerenciamento de cluster e o plano de trabalho para o usuário
+ Dimensionado automaticamente por padrão
+ Suporte para recursos de CPU e GPU
+ Permite o uso de VMs de baixa prioridade para redução de custos

A Computação do Azure Machine Learning pode ser criada no Python, pelo portal do Azure ou pela CLI. Precisa ser criada na região do seu workspace e não pode ser anexada a outro workspace. Esse destino de computação usa um contêiner do Docker para execução e empacota as dependências para replicar o mesmo ambiente em todos os nós.

> [!Warning]
> É recomendável criar um novo workspace para usar a Computação do Azure Machine Learning. Há uma possibilidade remota de que os usuários que tentarem criar a Computação do Azure Machine Learning em um workspace existente recebam um erro. A computação existente no workspace deve continuar a funcionar sem problemas.

### <a name="azure-machine-learning-sdk-for-python-v102"></a>SDK do Azure Machine Learning para Python v1.0.2
+ **Alterações de quebra**
  + Nesta versão, removemos o suporte para a criação de uma VM do Azure Machine Learning. Ainda é possível anexar uma VM em nuvem existente ou um servidor local remoto.
  + Também removemos o suporte para o IA do Lote, agora, o suporte para todos ocorrerá por meio da Computação do Azure Machine Learning.

+ **Novo**
  + Para pipelines do aprendizado de máquina:
    + [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimator_step.estimatorstep?view=azure-ml-py)
    + [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyper_drive_step.hyperdrivestep?view=azure-ml-py)
    + [MpiStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.mpi_step.mpistep?view=azure-ml-py)


+ **Atualizado**
  + Para pipelines do aprendizado de máquina:
    + [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) aceita runconfig agora
    + [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) agora copia de e para uma fonte de dados SQL
    + Agendar a funcionalidade no SDK para criar e atualizar agendas e executar pipelines publicados

<!--+ **Bugs fixed**-->

### <a name="azure-machine-learning-data-prep-sdk-v052"></a>SDK de preparação de dados do Azure Machine Learning v0.5.2
+ **Alterações de quebra**
  * `SummaryFunction.N` foi renomeado para `SummaryFunction.Count`.

+ **Correções de bugs**
  * Use o token de execução do AML mais recente ao ler e gravar em repositórios de armazenamento em execuções remotas. Anteriormente, se o Token de Execução do AML fosse atualizado no Python, o runtime de preparação de dados não seria atualizado com o novo Token de Execução do AML.
  * Mensagens de erro mais claras adicionais
  * to_spark_dataframe() não falhará mais quando o Spark usar a serialização `Kryo`
  * Agora, o Inspetor de Contagem de Valor pode mostrar mais de mil valores únicos
  * A divisão aleatória não falhará mais se o fluxo de origem original não tiver um nome

+ **Mais informações**
  * [SDK de preparação de dados do Azure Machine Learning](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>Documentos e notebooks
+ Pipelines de ML
  + Notebooks novos e atualizados para começar a ver exemplos de pipelines, escopo de lote e transferência de estilo: https://aka.ms/aml-pipeline-notebooks
  + Saiba como [criar seu primeiro pipeline](how-to-create-your-first-pipeline.md)
  + Saiba como [executar previsões em lotes usando pipelines](how-to-use-parallel-run-step.md)
+ Computação de destino do Azure Machine Learning
  + [Notebooks de exemplo](https://aka.ms/aml-notebooks) agora são atualizados para usar essa nova computação gerenciada.
  + [Saiba mais sobre essa computação](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Portal do Azure: novos recursos
+ Crie e gerencie tipos de [Computação do Azure Machine Learning](how-to-set-up-training-targets.md#amlcompute) no portal.
+ Monitore o uso de cota e a [cota de solicitações](how-to-manage-quotas.md) da Computação do Azure Machine Learning.
+ Veja o status do cluster da Computação do Machine Learning em tempo real.
+ Foi adicionado suporte à rede virtual para a criação de Computação do Azure Machine Learning e do Serviço de Kubernetes do Azure.
+ Execute novamente os pipelines publicados com parâmetros existentes.
+ Novos [gráficos de aprendizado de máquina automatizado](how-to-understand-automated-ml.md) para modelos de classificação (gráfico de importância de recursos de comparação de precisão e calibragem com explicabilidade de modelo) e modelos de regressão (resíduos e gráfico de importância de recursos com explicabilidade de modelo).
+ Os pipelines podem ser exibidos no portal do Azure




## <a name="2018-11-20"></a>20-11-2018

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>SDK do Azure Machine Learning para Python v0.1.80

+ **Alterações de quebra**
  * Namespace *azureml.Train.widgets* foi movido para *azureml.widgets*.
  * *azureml.core.compute.AmlCompute* substitui as seguintes classes - *azureml.core.compute.BatchAICompute* e *azureml.core.compute.DSVMCompute*. A última classe será removida nas versões subsequentes. A classe AmlCompute tem agora uma definição mais fácil e simplesmente precisa de um vm_size e do max_nodes e dimensionará automaticamente a seu cluster de 0 para o max_nodes quando um trabalho for enviado. Nossos [notebooks de exemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/training) foram atualizados com essas informações e devem fornecer exemplos de uso. Esperamos que você goste dessa simplificação e dos muitos dos recursos mais interessantes para ficar em uma versão posterior!

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>SDK de preparação de dados do AML v0.5.1

Saiba mais sobre o SDK de preparação de dados lendo [docs de referência](https://aka.ms/data-prep-sdk).
+ **Novos recursos**
   * Criada uma nova CLI DataPrep para executar pacotes DataPrep e exibir o perfil de dados para um conjunto de dados ou o fluxo de dados
   * API de SetColumnType reprojetado para melhorar a usabilidade
   * Smart_read_file renomeado para auto_read_file
   * Agora inclui distorção e curtose no Perfil de Dados
   * Pode criar amostra com amostragem estratificada
   * Pode ler de arquivos zip que contenham arquivos CSV
   * Pode dividir conjuntos de dados por linha com a divisão aleatória (por exemplo, nos conjuntos de teste-treinamento)
   * Pode obter os tipos de dados de coluna de um fluxo de dados ou um perfil de dados chamando `.dtypes`
   * Pode obter a contagem de linhas de um fluxo de dados ou um perfil de dados chamando `.row_count`

+ **Correções de bugs**
   * Corrigidos para conversão dupla
   * Ativo corrigido após qualquer adição de coluna
   * Corrigido um problema com FuzzyGrouping, onde ele não detectaria grupos em alguns casos
   * Função de classificação corrigida para respeitar a ordem de classificação de várias colunas
   * Fixos e/ou expressões para ser semelhante a como `pandas` lida com isso
   * Leitura corrigida do caminho dbfs
   * Mensagens de erro tornadas mais compreensíveis
   * Agora não apresenta mais falha ao ler em destino de computação remota usando um token AML
   * Agora não falhará na DSVM do Linux
   * Agora não falha quando os valores de cadeia de caracteres não são em predicados de cadeia de caracteres
   * Agora manipula erros de asserção ao fluxo de dados deve falhar corretamente
   * Agora dá suporte a locais de armazenamento dbutils montados no Azure Databricks

## <a name="2018-11-05"></a>05-11-2018

### <a name="azure-portal"></a>Portal do Azure
O portal do Azure para Azure Machine Learning tem as seguintes atualizações:
  * Uma nova **Pipelines** guia para pipelines publicados.
  * Adicionado suporte para anexar a um cluster HDInsight existente como um destino de computação.

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>SDK de aprendizado de máquina do AML para Python v0.1.74

+ **Alterações de quebra**
  * Workspace.compute_targets, datastores, experimentos, imagens, modelos e *webservices* são propriedades em vez de métodos. Por exemplo, substitua *Workspace.compute_targets()* com *Workspace.compute_targets*.
  * *Run.get_context* pretere *Run.get_submitted_run*. O último método será removido em versões subsequentes.
  * A classe *PipelineData* agora espera um objeto de armazenamento de dados como um parâmetro em vez de datastore_name. Da mesma forma, o *Pipeline* aceita default_datastore em vez de default_datastore_name.

+ **Novos recursos**
  * O caderno de amostras de [amostra do AML](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-mpi-batch-prediction.ipynb) agora usa etapas de MPI.
  * O widget RunDetails para notebooks Jupyter é atualizado para mostrar uma visualização do pipeline.

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>SDK de preparação de dados do AML v0.4.0

+ **Novos recursos**
  * Tipo de contagem adicionada ao perfil de dados
  * Contagem de valores e histograma estão agora disponíveis
  * Mais percentis no perfil de dados
  * Mediana está disponível em Summarize
  * Agora há suporte para Python 3.7
  * Quando você salva um fluxo de dados que contém datastores em um pacote DataPrep, as informações do armazenamento de dados serão mantidas como parte do pacote DataPrep
  * A gravação no armazenamento de dados agora é suportada

+ **Bug corrigido**
  * Extensões de inteiro não assinado de 64 bits agora são tratadas corretamente no Linux
  * Etiqueta de texto incorreta corrigida para arquivos de texto simples em smart_read
  * O tipo de coluna de cadeia de caracteres agora aparece na visualização de métricas
  * A contagem de tipos agora é fixada para mostrar ValueKinds mapeados para um único FieldType em vez de um individual
  * Write_to_csv não falha quando o caminho é fornecido como uma cadeia de caracteres
  * Ao usar Replace, deixar "localizar" em branco não falhará mais

## <a name="2018-10-12"></a>12-10-2018

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>SDK de aprendizado de máquina do Azure para Python v0.1.68

+ **Novos recursos**
  * Suporte a vários locatários ao criar novo workspace.

+ **Bugs corrigidos**
  * Você não precisa fixar a versão da biblioteca pynacl ao implantar o serviço web.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>SDK de preparação de dados de aprendizado de máquina do Azure v0.3.0

+ **Novos recursos**
  * Adicionado método transform_partition_with_file (script_path), que permite aos usuários passar no caminho de um arquivo Python para executar

## <a name="2018-10-01"></a>01-10-2018

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>SDK de aprendizado de máquina do Azure para Python v0.1.65
A [versão 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) inclui novos recursos, mais documentação, correções de bugs e mais [exemplos de blocos de notas](https://aka.ms/aml-notebooks).

Veja [a lista de problemas conhecidos](resource-known-issues.md) para aprender sobre erros e soluções conhecidas.

+ **Alterações de quebra**
  * Workspace.Experiments, Workspace.models, Workspace.compute_targets, Workspace.images, dicionário de retorno Workspace.web_services, retornado anteriormente de lista. Veja a documentação da API [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py).

  * Automatizados de aprendizado de máquina removido erro normalizado quadrada da média das métricas principais.

+ **HyperDrive**
  * Várias correções de bugs do HyperDrive para Bayesian, melhorias de desempenho para obter chamadas de métricas.
  * Atualização de Tensorflow 1.10 do 1.9
  * Otimização de imagem do Docker para inicialização a frio.
  * O trabalho agora reporta o status correto, mesmo se ele sair com um código de erro diferente de 0.
  * Validação de atributo RunConfig no SDK.
  * O objeto de execução HyperDrive suporta cancelamentos semelhantes a uma execução normal: não é necessário passar nenhum parâmetro.
  * Melhorias de widget para manter o estado dos valores suspensos para execuções distribuídas e execuções do HyperDrive.
  * TensorBoard e outros arquivos de log suportam fixos para o servidor Parameter.
  * Suporte ao Intel (R) MPI no lado do serviço.
  * Bugfix ao ajuste de parâmetro para correção de execução distribuída durante a validação no BatchAI.
  * O Context Manager agora identifica a instância primária.

+ **Experiência do portal do Azure**
  * log_table () e log_row () são suportados em detalhes da execução.
  * Crie automaticamente gráficos para tabelas e linhas com 1, 2 ou 3 colunas numéricas e uma coluna categórica opcional.

+ **Machine Learning automatizado**
  * Melhor tratamento e documentação de erros
  * Corrigidos problemas de desempenho de recuperação de propriedades de execução.
  * Fixo continuar a execução do problema.
  * Problemas :::no-loc text="ensembling"::: de iteração fixos.
  * Bug deslocada de treinamento fixo no MAC OS.
  * Curva média de solicitação de pull/ROC no cenário de validação personalizada da macro da resolução.
  * Removida a lógica extra de índice.
  * Removemos o filtro de get_output API.

+ **Pipelines**
  * Adicionado um método Pipeline.publish () para publicar um pipeline diretamente, sem precisar executar uma execução primeiro.
  * Adicionado um método PipelineRun.get_pipeline_runs () para buscar as execuções de pipeline que foram geradas a partir de um pipeline publicado.

+ **Project Brainwave**
  * Suporte atualizado para novos modelos de IA disponíveis em FPGAs.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>SDK de preparação de dados de aprendizado de máquina do Azure v0.2.0
[Versão 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) inclui recursos e correções de bug a seguir:

+ **Novos recursos**
  * Suporte para codificação one-hot
  * Suporte para a transformação de quantil

+ **Bug corrigido:**
  * Funciona com qualquer versão do Tornado, sem necessidade de rebaixar sua versão do Tornado
  * O valor conta para todos os valores, não apenas os três primeiros

## <a name="2018-09-public-preview-refresh"></a>09-2018 (atualização da visualização pública)

Uma nova versão atualizada do Azure Machine Learning: Leia mais sobre esta versão:https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/


## <a name="next-steps"></a>Próximas etapas

Leia a visão geral do [Azure Machine Learning](overview-what-is-azure-ml.md).
