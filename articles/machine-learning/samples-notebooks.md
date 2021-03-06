---
title: Exemplos de Notebooks Jupyter
titleSuffix: Azure Machine Learning
description: Encontre e use Jupyter Notebooks de exemplo para explorar o SDK do Python do Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: 7242b82ee5c43878a33731bd1f02b685020f22b0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78673649"
---
# <a name="explore-azure-machine-learning-with-jupyter-notebooks"></a>Explorar o Azure Machine Learning com os Jupyter Notebooks

O [exemplo de repositório de Notebooks do Azure Machine Learning](https://github.com/azure/machinelearningnotebooks) inclui as amostras mais recentes do SDK do Python do Azure Machine Learning. Esses notebooks Jupyter foram projetados para ajudá-lo a explorar o SDK e servem como modelos para seus próprios projetos de aprendizado de máquina.

Este artigo mostra como acessar o repositório nos seguintes ambientes:

- [Instância de computação do Azure Machine Learning](#notebookvm)
- [Trazer seu próprio servidor de notebook](#byo)
- [Máquina Virtual de Ciência de Dados](#dsvm)

> [!NOTE]
> Depois de clonar o repositório, você encontrará notebooks do tutorial na pasta **tutorials** e notebooks específicos do recurso na pasta **how-to-use-azureml**.

<a name="notebookvm"></a>
## <a name="get-samples-on-azure-machine-learning-compute-instance"></a>Obter amostras na instância de computação do Azure Machine Learning

A maneira mais fácil de começar a usar as amostras é concluir o [Tutorial: configurar o ambiente e o workspace](tutorial-1st-experiment-sdk-setup.md). Após a conclusão, você terá um servidor de notebook dedicado pré-carregado com o SDK e o repositório de exemplo. Não há necessidade de downloads nem de instalação.

<a name="byo"></a>

## <a name="get-samples-on-your-notebook-server"></a>Obter amostras em seu servidor de notebook

Caso deseje trazer seu próprio servidor de notebook para o desenvolvimento local, siga estas etapas:

[!INCLUDE [aml-your-server](../../includes/aml-your-server.md)]

Essas instruções instalam os pacotes base do SDK necessários para o início rápido e os notebooks do tutorial. Outros notebooks de exemplo podem exigir a instalação de componentes extras. Para obter mais informações, confira [Instalar o SDK do Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/install).

<a name="dsvm"></a>
## <a name="get-samples-on-dsvm"></a>Obter exemplos no DSVM

A DSVM (Máquina Virtual de Ciência de Dados) é uma imagem de VM personalizada, especificamente criada para ciência de dados. Se você [criar uma DSVM](how-to-configure-environment.md#dsvm), o SDK e o servidor de notebook serão instalados e configurados para você. No entanto, você ainda precisará criar um workspace e clonar o repositório de exemplo.

[!INCLUDE [aml-dsvm-server](../../includes/aml-dsvm-server.md)]

## <a name="next-steps"></a>Próximas etapas

Explore os [notebooks de exemplo](https://aka.ms/aml-notebooks) para descobrir o que o Azure Machine Learning pode fazer ou experimente estes tutoriais:

- [Treinar e implantar um modelo de classificação de imagem com MNIST](tutorial-train-models-with-aml.md)

- [Preparar os dados e usar aprendizado de máquina automatizado para treinar um modelo de regressão com o conjunto de dados de táxi de NYC](tutorial-auto-train-models.md)
