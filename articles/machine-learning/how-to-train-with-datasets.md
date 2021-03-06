---
title: Treinar com o azureml – conjuntos de valores
titleSuffix: Azure Machine Learning
description: Saiba como usar conjuntos de valores em treinamento
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 03/09/2020
ms.openlocfilehash: 401383f2d483836bf725051810d78167869f7b22
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79283492"
---
# <a name="train-with-datasets-in-azure-machine-learning"></a>Treine com conjuntos de os Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Neste artigo, você aprenderá as duas maneiras de consumir [Azure Machine Learning conjuntos](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py) de dados em um treinamento de experimento remoto é executado sem se preocupar com cadeias de conexão ou caminhos.

- Opção 1: se você tiver dados estruturados, crie um TabularDataset e use-o diretamente em seu script de treinamento.

- Opção 2: se você tiver dados não estruturados, crie um filedataset e monte ou baixe arquivos para uma computação remota para treinamento.

Azure Machine Learning conjuntos de valores fornecem uma integração direta com Azure Machine Learning produtos de treinamento como [ScriptRun](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrun?view=azure-ml-py), [estimador](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py), [HyperDrive](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive?view=azure-ml-py) e [pipelines de Azure Machine Learning](how-to-create-your-first-pipeline.md).

## <a name="prerequisites"></a>Pré-requisitos

Para criar e treinar com conjuntos de os, você precisa:

* Uma assinatura do Azure. Caso não tenha uma assinatura do Azure, crie uma conta gratuita antes de começar. Experimente hoje mesmo a [versão gratuita ou paga do Azure Machine Learning](https://aka.ms/AMLFree).

* Um [espaço de trabalho Azure Machine Learning](how-to-manage-workspace.md).

* O [SDK do Azure Machine Learning para Python instalado](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), que inclui o pacote de conjuntos de linhas do azureml.

> [!Note]
> Algumas classes de conjunto de objetos têm dependências no pacote [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) . Para usuários do Linux, essas classes têm suporte apenas nas seguintes distribuições: Red Hat Enterprise Linux, Ubuntu, Fedora e CentOS.

## <a name="option-1-use-datasets-directly-in-training-scripts"></a>Opção 1: usar conjuntos de valores diretamente em scripts de treinamento

Neste exemplo, você cria um [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) e o usa como uma entrada direta para seu `estimator` objeto para treinamento. 

### <a name="create-a-tabulardataset"></a>Criar um TabularDataset

O código a seguir cria um TabularDataset não registrado de uma URL da Web. Você também pode criar conjuntos de valores de arquivos locais ou caminhos em repositórios de armazenamento. Saiba mais sobre [como criar conjuntos de](https://aka.ms/azureml/howto/createdatasets)os.

```Python
from azureml.core.dataset import Dataset

web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)
```

### <a name="access-the-input-dataset-in-your-training-script"></a>Acessar o conjunto de dados de entrada em seu script de treinamento

Os objetos TabularDataset fornecem a capacidade de carregar os dados em um data frame pandas ou Spark para que você possa trabalhar com bibliotecas de treinamento e preparação de dados familiares. Para aproveitar esse recurso, você pode passar um TabularDataset como a entrada em sua configuração de treinamento e, em seguida, recuperá-lo em seu script.

Para fazer isso, acesse o conjunto de dados [`Run`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py) de entrada por meio do objeto em seu [`to_pandas_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset#to-pandas-dataframe-on-error--null---out-of-range-datetime--null--) script de treinamento e use o método. 

```Python
%%writefile $script_folder/train_titanic.py

from azureml.core import Dataset, Run

run = Run.get_context()
# get the input dataset by name
dataset = run.input_datasets['titanic']
# load the TabularDataset to pandas DataFrame
df = dataset.to_pandas_dataframe()
```

### <a name="configure-the-estimator"></a>Configurar o estimador

Um objeto [estimador](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) é usado para enviar a execução do experimento. Azure Machine Learning tem estimadores pré-configurados para estruturas de aprendizado de máquina comuns, bem como um estimador genérico.

Esse código cria um objeto estimador genérico `est`,, que especifica

* Um diretório de script para seus scripts. Todos os arquivos neste diretório são carregados nos nós do cluster para execução.
* O script de treinamento, *train_titanic. py*.
* O conjunto de dados de entrada `titanic`para treinamento,. `as_named_input()`é necessário para que o conjunto de dados de entrada possa ser referenciado pelo nome atribuído em seu script de treinamento. 
* O destino de computação para o experimento.
* A definição de ambiente para o experimento.

```Python
est = Estimator(source_directory=script_folder,
                entry_script='train_titanic.py',
                # pass dataset object as an input with name 'titanic'
                inputs=[titanic_ds.as_named_input('titanic')],
                compute_target=compute_target,
                environment_definition= conda_env)

# Submit the estimator as part of your experiment run
experiment_run = experiment.submit(est)
experiment_run.wait_for_completion(show_output=True)
```


## <a name="option-2--mount-files-to-a-remote-compute-target"></a>Opção 2: montar arquivos em um destino de computação remota

Se você desejar disponibilizar seus arquivos de dados no destino de computação para treinamento, use [Filedataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) para montar ou baixar arquivos referenciados por ele.

### <a name="mount-vs-download"></a>Montagem vs. download

Há suporte para a montagem ou o download de arquivos de qualquer formato para conjuntos de dados criados a partir do armazenamento de BLOBs do Azure, arquivos do Azure, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2 do Azure SQL e banco de dados do Azure para PostgreSQL. 

Ao montar um conjunto de um DataSet, você anexa os arquivos referenciados pelo conjunto de um diretório (ponto de montagem) e os disponibiliza no destino de computação. A montagem tem suporte para computações baseadas em Linux, incluindo Azure Machine Learning computação, máquinas virtuais e HDInsight. Quando você baixa um conjunto de um DataSet, todos os arquivos referenciados pelo conjunto de um serão baixados para o destino de computação. O download tem suporte para todos os tipos de computação. 

Se o seu script processa todos os arquivos referenciados pelo conjunto de dados e seu disco de computação pode se ajustar a seu conjunto de dados completo, o download é recomendado para evitar a sobrecarga de streaming de serviços de armazenamento. Se o tamanho dos dados exceder o tamanho do disco de computação, o download não será possível. Para este cenário, recomendamos a montagem, pois apenas os arquivos de dados usados pelo script são carregados no momento do processamento.

O código a seguir é `dataset` montado no diretório Temp em`mounted_path`

```python
import tempfile
mounted_path = tempfile.mkdtemp()

# mount dataset onto the mounted_path of a Linux-based compute
mount_context = dataset.mount(mounted_path)

mount_context.start()

import os
print(os.listdir(mounted_path))
print (mounted_path)
```

### <a name="create-a-filedataset"></a>Criar um FileDataset

O exemplo a seguir cria um filedataset não registrado a partir de URLs da Web. Saiba mais sobre [como criar conjuntos](https://aka.ms/azureml/howto/createdatasets) de informações de outras fontes.

```Python
from azureml.core.dataset import Dataset

web_paths = [
            'http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz'
            ]
mnist_ds = Dataset.File.from_files(path = web_paths)
```

### <a name="configure-the-estimator"></a>Configurar o estimador

Além de passar o conjunto de `inputs` dados por meio do parâmetro no estimador, você também pode `script_params` passar o conjunto de dados por meio de e obter o caminho do dado (ponto de montagem) em seu script de treinamento por meio de argumentos. Dessa forma, você pode manter seu script de treinamento independente do azureml-SDK. Em outras palavras, você poderá usar o mesmo script de treinamento para depuração local e treinamento remoto em qualquer plataforma de nuvem.

Um objeto de estimador [SKLearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) é usado para enviar a execução para experimentos de scikit-learn. Saiba mais sobre o treinamento com o [estimador do SKlearn](how-to-train-scikit-learn.md).

```Python
from azureml.train.sklearn import SKLearn

script_params = {
    # mount the dataset on the remote compute and pass the mounted path as an argument to the training script
    '--data-folder': mnist_ds.as_named_input('mnist').as_mount(),
    '--regularization': 0.5
}

est = SKLearn(source_directory=script_folder,
              script_params=script_params,
              compute_target=compute_target,
              environment_definition=env,
              entry_script='train_mnist.py')

# Run the experiment
run = experiment.submit(est)
run.wait_for_completion(show_output=True)
```

### <a name="retrieve-the-data-in-your-training-script"></a>Recuperar os dados em seu script de treinamento

Depois que você enviar a execução, os arquivos de `mnist` dados referenciados pelo DataSet serão montados no destino de computação. O código a seguir mostra como recuperar os dados em seu script.

```Python
%%writefile $script_folder/train_mnist.py

import argparse
import os
import numpy as np
import glob

from utils import load_data

# retrieve the 2 arguments configured through script_params in estimator
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = args.data_folder
print('Data folder:', data_folder)

# get the file paths on the compute
X_train_path = glob.glob(os.path.join(data_folder, '**/train-images-idx3-ubyte.gz'), recursive=True)[0]
X_test_path = glob.glob(os.path.join(data_folder, '**/t10k-images-idx3-ubyte.gz'), recursive=True)[0]
y_train_path = glob.glob(os.path.join(data_folder, '**/train-labels-idx1-ubyte.gz'), recursive=True)[0]
y_test = glob.glob(os.path.join(data_folder, '**/t10k-labels-idx1-ubyte.gz'), recursive=True)[0]

# load train and test set into numpy arrays
X_train = load_data(X_train_path, False) / 255.0
X_test = load_data(X_test_path, False) / 255.0
y_train = load_data(y_train_path, True).reshape(-1)
y_test = load_data(y_test, True).reshape(-1)
```

## <a name="notebook-examples"></a>Exemplos de notebook

Os [notebooks de conjunto de anotações](https://aka.ms/dataset-tutorial) demonstram e se expandem sobre os conceitos deste artigo.

## <a name="next-steps"></a>Próximas etapas

* [Treinar automaticamente modelos de aprendizado de máquina](how-to-auto-train-remote.md) com o TabularDatasets

* [Treinar modelos de classificação de imagem](https://aka.ms/filedataset-samplenotebook) com DataSets

* [Treinar com conjuntos de valores usando pipelines](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/pipeline-with-datasets/pipeline-for-image-classification.ipynb)
