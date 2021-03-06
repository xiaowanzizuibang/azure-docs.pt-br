---
title: Como consultar logs da solução iniciar/parar VMs
description: Este artigo descreve como consultar dados de log gerados pela solução iniciar/parar VMs de Azure Monitor.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 472f3762ca18f71ba95053576daf025d8477fee9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81604738"
---
# <a name="how-to-query-logs-from-startstop-vms-solution"></a>Como consultar logs da solução iniciar/parar VMs

A automação do Azure encaminha dois tipos de registros para o espaço de trabalho Log Analytics vinculado: logs de trabalho e fluxos de trabalho. Esses dados estão disponíveis para [consulta](../azure-monitor/log-query/log-query-overview.md) no Azure monitor.

## <a name="job-logs"></a>Logs de trabalho

|Propriedade | Descrição|
|----------|----------|
|Chamador |  Quem iniciou a operação. Os valores possíveis são um endereço de email ou o sistema para trabalhos agendados.|
|Categoria | Classificação do tipo de dados. Para a Automação, o valor é JobLogs.|
|CorrelationId | O GUID que é a ID de correlação do trabalho de runbook.|
|JobId | GUID que é a ID do trabalho de runbook.|
|operationName | Especifica o tipo de operação realizada no Azure. Para a Automação, o valor é Job.|
|resourceId | Especifica o tipo de recurso no Azure. Para a Automação, o valor é a conta da Automação associada ao runbook.|
|ResourceGroup | Especifica o nome do grupo de recursos do trabalho do runbook.|
|ResourceProvider | Especifica o serviço do Azure que fornece os recursos que você pode implantar e gerenciar. Para Automação, o valor é Automação do Azure.|
|ResourceType | Especifica o tipo de recurso no Azure. Para a Automação, o valor é a conta da Automação associada ao runbook.|
|resultType | O status do trabalho de runbook. Os valores possíveis são:<br>- Iniciado<br>- Parado<br>- Suspenso<br>- Com falha<br>- Êxito|
|resultDescription | Descreve o estado de resultado do trabalho de runbook. Os valores possíveis são:<br>- O trabalho foi iniciado<br>- O trabalho falhou<br>- Trabalho Concluído|
|RunbookName | Especifica o nome do runbook.|
|SourceSystem | Especifica o sistema de origem dos dados enviados. Para a Automação, o valor é OpsManager|
|StreamType | Especifica o tipo de evento. Os valores possíveis são:<br>- Detalhado<br>- Saída<br>- Erro<br>- Aviso|
|SubscriptionId | Especifica a ID da assinatura do trabalho.
|Hora | Data e hora da execução do trabalho de runbook.|

## <a name="job-streams"></a>Transmissões de trabalho

|Propriedade | Descrição|
|----------|----------|
|Chamador |  Quem iniciou a operação. Os valores possíveis são um endereço de email ou o sistema para trabalhos agendados.|
|Categoria | Classificação do tipo de dados. Para a Automação, o valor é JobStreams.|
|JobId | GUID que é a ID do trabalho de runbook.|
|operationName | Especifica o tipo de operação realizada no Azure. Para a Automação, o valor é Job.|
|ResourceGroup | Especifica o nome do grupo de recursos do trabalho do runbook.|
|resourceId | Especifica o tipo de ID de recurso no Azure. Para a Automação, o valor é a conta da Automação associada ao runbook.|
|ResourceProvider | Especifica o serviço do Azure que fornece os recursos que você pode implantar e gerenciar. Para Automação, o valor é Automação do Azure.|
|ResourceType | Especifica o tipo de recurso no Azure. Para a Automação, o valor é a conta da Automação associada ao runbook.|
|resultType | O resultado do trabalho de runbook no momento em que o evento foi gerado. Um valor possível é:<br>- InProgress|
|resultDescription | Inclui o fluxo de saída do runbook.|
|RunbookName | O nome do runbook.|
|SourceSystem | Especifica o sistema de origem dos dados enviados. Para a Automação, o valor é OpsManager.|
|StreamType | O tipo de fluxo de trabalho. Os valores possíveis são:<br>- Andamento<br>- Saída<br>- Aviso<br>- Erro<br>- Depurar<br>- Detalhado|
|Hora | Data e hora da execução do trabalho de runbook.|

Quando você executa uma pesquisa de logs que retorna registros da categoria de **JobLogs** ou **JobStreams**, pode selecionar a exibição **JobLogs** ou **JobStreams**, que exibe um conjunto de blocos resumindo as atualizações retornadas pela pesquisa.

## <a name="sample-log-searches"></a>Pesquisas de log de exemplo

A tabela a seguir fornece pesquisas de log de exemplo para os registros de alerta coletados por essa solução.

|Consulta | Descrição|
|----------|----------|
|Localizar trabalhos para o runbook ScheduledStartStop_Parent que foram finalizados com êxito | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Localizar trabalhos para ScheduledStartStop_Parent de runbook que não foram concluídos com êxito | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Failed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Localizar trabalhos para o runbook SequencedStartStop_Parent que foram finalizados com êxito | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Localizar trabalhos para SequencedStartStop_Parent de runbook que não foram concluídos com êxito | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Failed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|

## <a name="next-steps"></a>Próximas etapas

A solução **iniciar/parar VMs fora do horário comercial** não inclui um conjunto predefinido de alertas. Examine os [alertas de criar log](../azure-monitor/platform/alerts-log.md) com Azure monitor para saber como criar alertas de falha de trabalho para dar suporte aos processos e procedimentos operacionais ou DevOps.