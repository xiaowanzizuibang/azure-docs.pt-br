---
title: Configurar Logs e métricas de nuvem para o gateway auto-hospedado do gerenciamento de API do Azure | Microsoft Docs
description: Saiba como configurar logs e métricas de nuvem para o gateway auto-hospedado do gerenciamento de API do Azure
services: api-management
documentationcenter: ''
author: miaojiang
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/30/2020
ms.author: apimpm
ms.openlocfilehash: d0fbdcb877e91a703306f15fdc7507fd19d534f4
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82205125"
---
# <a name="configure-cloud-metrics-and-logs-for-azure-api-management-self-hosted-gateway"></a>Configurar Logs e métricas de nuvem para o gateway auto-hospedado do gerenciamento de API do Azure

Este artigo fornece detalhes para configurar logs e métricas de nuvem para o [Gateway auto-hospedado](./self-hosted-gateway-overview.md).

O gateway auto-hospedado deve ser associado a um serviço de gerenciamento de API e requer conectividade TCP/IP de saída para o Azure na porta 443. O gateway aproveita a conexão de saída para enviar telemetria ao Azure, se configurado para fazer isso. 

## <a name="metrics"></a>Métricas
Por padrão, o gateway auto-hospedado emite um número de métricas por meio de [Azure monitor](https://azure.microsoft.com/services/monitor/), o mesmo que o gateway gerenciado [na nuvem](api-management-howto-use-azure-monitor.md). 

O recurso pode ser habilitado ou desabilitado usando `telemetry.metrics.cloud` a chave no ConfigMap da implantação do gateway. Abaixo está uma análise das configurações disponíveis:

| Campo  | Padrão | Descrição |
| ------------- | ------------- | ------------- |
| telemetria. métricas. nuvem  | `true` | Habilita o registro em log por meio de Azure Monitor. O valor pode `true`ser `false`,. |


Aqui está uma configuração de exemplo:

```yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
        name: contoso-gateway-environment
    data:
        config.service.endpoint: "<contoso-gateway-management-endpoint>"
        telemetry.metrics.cloud: "true"
```

O gateway auto-hospedado emite atualmente as seguintes métricas por meio do Azure Monitor:

| Métrica  | Descrição |
| ------------- | ------------- |
| Requests  | Número de solicitações de API no período |
| Duração das solicitações de gateway | Número de milissegundos do momento em que o gateway recebeu a solicitação até o momento em que a resposta foi enviada por completo |
| Duração de solicitações de back-end | Número de milissegundos gastos na E/S do back-end no total (conectando, enviando e recebendo bytes)  |

## <a name="logs"></a>Logs

Atualmente, o gateway auto-hospedado não envia [logs de diagnóstico](https://docs.microsoft.com/azure/api-management/api-management-howto-use-azure-monitor#diagnostic-logs) para a nuvem. No entanto, é possível [configurar e persistir os logs localmente](how-to-configure-local-metrics-logs.md) onde o gateway auto-hospedado é implantado. 

Se um gateway for implantado no [serviço kubernetes do Azure](https://azure.microsoft.com/services/kubernetes-service/), você poderá habilitar [Azure monitor para contêineres](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview) para coletar logs de seus contêineres e exibi-los no log Analytics. 


## <a name="next-steps"></a>Próximas etapas

* Para saber mais sobre o gateway auto-hospedado, confira [visão geral do gateway de gerenciamento de API do Azure-hospedado](self-hosted-gateway-overview.md)
* Saiba como [configurar e persistir logs localmente](how-to-configure-local-metrics-logs.md)


