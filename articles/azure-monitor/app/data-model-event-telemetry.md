---
title: Modelo de dados do Azure Application Insights Telemetry – telemetria de eventos | Microsoft Docs
description: Modelo de dados do Application Insights para telemetria de eventos
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: bd8b2581f7642f6825aaf0d1b51c8e94d4333d33
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77671873"
---
# <a name="event-telemetry-application-insights-data-model"></a>Telemetria de eventos: modelo de dados do Application Insights

Você pode criar itens de telemetria do evento (em [Application Insights](../../azure-monitor/app/app-insights-overview.md)) para representar um evento que ocorreu em seu aplicativo. Geralmente trata-se de uma interação do usuário como um clique de botão ou finalização de compra. Também pode ser um evento de ciclo de vida do aplicativo como a inicialização ou atualização de configuração. 

Semanticamente, eventos podem ou não ser correlacionados às solicitações. No entanto, se usada corretamente, a telemetria de eventos é mais importante que solicitações ou rastreamentos. Os eventos representam a telemetria de negócios e devem ser um assunto para uma [amostragem](../../azure-monitor/app/api-filtering-sampling.md)separada e menos agressiva.

## <a name="name"></a>Name

Nome do evento. Para permitir o agrupamento adequado e métricas úteis, restrinja seu aplicativo de maneira que ele gere um pequeno número de nomes de eventos separados. Por exemplo, não use um nome à parte para cada instância gerado de um evento.

Comprimento máximo: 512 caracteres

## <a name="custom-properties"></a>Propriedades personalizadas

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Medidas personalizadas

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Próximas etapas

- Consulte [modelo de dados](data-model.md) para modelo de dados e tipos do Application Insights.
- [Escrever telemetria do evento personalizada](../../azure-monitor/app/api-custom-events-metrics.md#trackevent)
- Confira as [plataformas](../../azure-monitor/app/platforms.md) com suporte do Application Insights.
