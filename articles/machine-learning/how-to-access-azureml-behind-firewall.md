---
title: Usar Azure Machine Learning atrás de um firewall
titleSuffix: Azure Machine Learning
description: Use com segurança Azure Machine Learning por trás do firewall do Azure. Saiba mais sobre os hosts que você deve permitir por meio do firewall para que Azure Machine Learning funcionem corretamente.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 04/27/2020
ms.openlocfilehash: 2c5fe9d46ef408775cc4b99d4820a652a91683c4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82196316"
---
# <a name="use-azure-machine-learning-workspace-behind-azure-firewall"></a>Usar Azure Machine Learning espaço de trabalho por trás do firewall do Azure

Este artigo contém informações sobre como configurar o Firewall do Azure para uso com o Azure Machine Learning.

O Firewall do Azure pode ser usado para controlar o acesso ao seu espaço de trabalho do Azure Machine Learning e à Internet pública. Se não estiver configurado corretamente, o firewall poderá causar problemas ao usar seu espaço de trabalho.

## <a name="network-rules"></a>Regras de rede

No firewall, crie uma regra de rede permitindo o tráfego de e para os endereços neste artigo.

> [!TIP]
> Ao adicionar a regra de rede, defina o __protocolo__ como qualquer e as portas para `*`.
>
> Para obter mais informações sobre como configurar o Firewall do Azure, consulte [implantar e configurar o Firewall do Azure](../firewall/tutorial-firewall-deploy-portal.md#configure-a-network-rule).

## <a name="microsoft-hosts"></a>Hosts da Microsoft

Os hosts nesta seção são de propriedade da Microsoft e fornecem os serviços necessários para o funcionamento adequado do seu espaço de trabalho.

| **Nome do host** | **Objetivo** |
| ---- | ---- |
| **\*. batchai.core.windows.net** | Clusters de treinamento |
| **ml.azure.com** | Azure Machine Learning Studio |
| **\*. azureml.ms** | Usado por APIs de Azure Machine Learning |
| **\*. experiments.azureml.net** | Usado por experimentos em execução no Azure Machine Learning|
| **\*. modelmanagement.azureml.net** | Usado para registrar e implantar modelos|
| **mlworkspace.azure.ai** | Usado pelo portal do Azure ao exibir um espaço de trabalho |
| **\*. aether.ms** | Usado ao executar pipelines de Azure Machine Learning |
| **\*. instances.azureml.net** | Azure Machine Learning instâncias de computação |
| **windows.net** | Armazenamento do Blobs do Azure |
| **vault.azure.net** | Cofre de Chave do Azure |
| **microsoft.com** | Imagens base do Docker |
| **azurecr.io** | Registro de Contêiner do Azure |

## <a name="python-hosts"></a>Hosts do Python

Os hosts nesta seção são usados para instalar pacotes do Python. Eles são necessários durante o desenvolvimento, o treinamento e a implantação. 

| **Nome do host** | **Objetivo** |
| ---- | ---- |
| **anaconda.com** | Usado ao instalar pacotes do Conda |
| **pypi.org** | Usado ao instalar pacotes Pip |

## <a name="r-hosts"></a>Hosts de R

Os hosts nesta seção são usados para instalar pacotes do R. Eles são necessários durante o desenvolvimento, o treinamento e a implantação.

> [!IMPORTANT]
> Internamente, o SDK do R para Azure Machine Learning usa pacotes do Python. Portanto, você também deve permitir hosts Python por meio do firewall.

| **Nome do host** | **Objetivo** |
| ---- | ---- |
| **cloud.r-project.org** | Usado ao instalar pacotes do CRAN. |

Próximas etapas

* [[Implantar e configurar o Firewall do Azure](../firewall/tutorial-firewall-deploy-portal.md)]
* [Proteger trabalhos de experimentação e de inferência do Azure ML em uma rede virtual do Azure](how-to-enable-virtual-network.md)