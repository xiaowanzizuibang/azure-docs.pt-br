---
title: Publicar uma oferta de máquina virtual no Azure Marketplace
description: Lista as etapas necessárias para publicar uma oferta de máquina virtual existente no Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: dsindona
ms.openlocfilehash: f66ce0c15e976898d5022bf5705a82fe0969ec07
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82147651"
---
# <a name="publish-a-virtual-machine-offer"></a>Publicar uma oferta de máquina virtual

> [!IMPORTANT]
> A partir de 13 de abril de 2020, começaremos a mover o gerenciamento de suas ofertas de máquina virtual do Azure para o Partner Center. Após a migração, você criará e gerenciará suas ofertas no Partner Center. Siga as instruções em [criar uma máquina virtual do Azure oferta](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) para gerenciar suas ofertas migradas.

 A última etapa, depois de definir a oferta no portal e criar os recursos técnicos associados, é enviar a oferta para publicação. O diagrama a seguir descreve as principais etapas do processo de publicação para "entrar em operação":

![Etapas de publicação para a máquina virtual da oferta](./media/publishvm_013.png)

A tabela a seguir descreve essas etapas e fornece uma estimativa de tempo máximo para sua conclusão:
<!-- we need to tell them that if an offer seems stuck in a step, to know that they should file a support ticket (link to support ticket doc) -->


|  **Etapa de publicação**           | **Momento**    | **Descrição**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Validar os pré-requisitos         | 15 min   | Informações de oferta e configurações de oferta são validadas.                        |
| Teste de unidade validação (opcional) | 2 horas | Se você tiver selecionado para habilitar o Test Drive, a Microsoft valida a configuração do Test Drive, sua implantação e replicação através das regiões selecionadas. |
| Certificação                  | 3 dias | A oferta é analisada pela equipe de certificação do Azure. Esta etapa realizará varreduras por vírus, malware, conformidade de segurança e problemas de segurança. Comentários serão fornecidos se um problema for encontrado. |
| Provisionamento                   | 4 dias   | A oferta de VM é replicada nos sistemas de produção de mercado.               |
| Registro de embalagem e geração de leads | \< 1 hora  | Os ativos técnicos da oferta são empacotados para uso do cliente e os sistemas de Lead são configurados e definidos. |
|  Aprovação do publicador             |  -        | Revisão final do editor e confirmação antes que a oferta seja publicada. Você pode implantar sua oferta nas assinaturas selecionadas (nas etapas a informações de oferta) para verificar se ele atende a todos os seus requisitos.  |
| Provisionamento                   | 4 dias | A oferta de VM finalizada é replicada em sistemas e regiões de produção de mercado. | 
| Live                           | 4 dias | A oferta de VM é lançada, replicada para as regiões necessárias e disponibilizada ao público. |
|  |  |

Permitem até 16 dias para a conclusão desse processo.  Depois que você percorrer essas etapas de publicação, sua oferta de VM será listada na [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/). 

