---
title: Passo a passo do parceiro de Serviço de Emparelhamento
titleSuffix: Azure
description: Passo a passo do parceiro de Serviço de Emparelhamento
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: d626d98613bd5d988b599d0980c885d7f73bb958
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76720241"
---
# <a name="peering-service-partner-walkthrough"></a>Passo a passo do parceiro de Serviço de Emparelhamento

Esta seção explica as etapas que um provedor precisa seguir para habilitar um emparelhamento direto para o serviço de emparelhamento.

## <a name="create-direct-peering-connection-for-peering-service"></a>Criar conexão de emparelhamento direto para o serviço de emparelhamento
Os provedores de serviços podem expandir seu alcance geográfico criando um novo emparelhamento direto que dá suporte ao serviço de emparelhamento. Para fazer isso,
1. Se tornar um parceiro de serviço de emparelhamento, se ainda não estiver
1. Siga as instruções para [criar ou modificar um emparelhamento direto usando o portal](howto-direct-portal.md). Verifique se ele atende aos requisitos de alta disponibilidade.
1. Em seguida, siga as etapas para [habilitar o serviço de emparelhamento em um emparelhamento direto usando o portal](howto-peering-service-portal.md).

## <a name="use-legacy-direct-peering-connection-for-peering-service"></a>Usar conexão de emparelhamento direto herdada para serviço de emparelhamento
Se você tiver um emparelhamento direto herdado que deseja usar para dar suporte ao serviço de emparelhamento,
1. Torne-se um parceiro de serviço de emparelhamento, se ainda não estiver.
1. Siga as instruções para [converter um emparelhamento direto herdado para o recurso do Azure usando o portal](howto-legacy-direct-portal.md). Se necessário, solicite circuitos adicionais para atender aos requisitos de alta disponibilidade.
1. Em seguida, siga as etapas para [habilitar o serviço de emparelhamento em um emparelhamento direto usando o portal](howto-peering-service-portal.md).

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre a [política de emparelhamento](https://peering.azurewebsites.net/peering).
* Para saber mais sobre as etapas para configurar o emparelhamento direto com a Microsoft, siga [a explicação do emparelhamento direto](walkthrough-direct-all.md).
* Para saber mais sobre as etapas para configurar o emparelhamento do Exchange com a Microsoft, siga [a explicação do emparelhamento do Exchange](walkthrough-exchange-all.md).