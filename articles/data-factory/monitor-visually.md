---
title: Monitorar visualmente Azure Data Factory
description: Saiba como monitorar visualmente as fábricas de dados do Azure
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/19/2018
ms.openlocfilehash: 40b1b8d040c4b3ea76372920f88551fba35c5f26
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81419435"
---
# <a name="visually-monitor-azure-data-factory"></a>Monitorar visualmente Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Depois de criar e publicar um pipeline no Azure Data Factory, você pode associá-lo a um gatilho ou iniciar manualmente uma execução ad hoc. Você pode monitorar todas as execuções de pipeline nativamente no Azure Data Factory experiência do usuário. Para abrir a experiência de monitoramento, selecione o bloco **monitorar & gerenciar** na folha data factory do [portal do Azure](https://portal.azure.com/). Se você já estiver na UX do ADF, clique no ícone do **Monitor** na barra lateral esquerda.

Todas as execuções de data factory são exibidas no fuso horário local do navegador. Se você alterar o fuso horário, todos os campos de data/hora se ajustarão para aquele que você selecionou.

## <a name="monitor-pipeline-runs"></a>Monitorar execuções de pipeline

O modo de exibição de monitoramento padrão é a lista de execuções de pipeline no período de tempo selecionado. As seguintes colunas são exibidas:

| **Nome da coluna** | **Descrição** |
| --- | --- |
| Nome do Pipeline | Nome do pipeline |
| Ações | Ícones que permitem exibir detalhes da atividade, cancelar ou executar novamente o pipeline |
| Início da Execução | Data e hora de início para a execução do pipeline (MM/DD/AAAA, HH: MM: SS AM/PM) |
| Duration | Duração da execução (HH:MM:SS) |
| Disparado por | O nome do gatilho que iniciou o pipeline |
| Status | **Com falha, com** **êxito**, **em andamento**, **cancelado**ou **na fila** |
| Anotações | Marcas filtráveis associadas a um pipeline  |
| Parâmetros | Parâmetros para a execução do pipeline (pares de nome/valor) |
| Erro do | Se o pipeline falhou, o erro de execução |
| ID da execução | ID da execução de pipeline |

![Exibição de lista para monitoramento de execuções de pipeline](media/monitor-visually/pipeline-runs.png)

Você precisa selecionar manualmente o botão **Atualizar** para atualizar a lista de execuções de pipeline e de atividade. No momento, não há suporte para a atualização autoatual.

![Botão Atualizar](media/monitor-visually/refresh.png)

## <a name="monitor-activity-runs"></a>Monitorar execuções de atividade

Para exibir as execuções de atividade para cada execução de pipeline, selecione o ícone **Exibir execuções de atividade** na coluna **ações** . A exibição de lista mostra as execuções de atividade que correspondem a cada execução de pipeline.

| **Nome da coluna** | **Descrição** |
| --- | --- |
| Nome da atividade | Nome da atividade dentro do pipeline |
| Tipo de atividade | Tipo de atividade, como **Copy**, **ExecuteDataFlow**ou **AzureMLExecutePipeline** |
| Ações | Ícones que permitem que você veja informações de entrada JSON, informações de saída JSON ou experiências detalhadas de monitoramento específicas da atividade | 
| Início da Execução | Data e hora de início para a execução da atividade (MM/DD/AAAA, HH: MM: SS AM/PM) |
| Duration | Duração da execução (HH:MM:SS) |
| Status | **Com falha, com** **êxito**, **em andamento**ou **cancelado** |
| Integration Runtime | Em que Integration Runtime a atividade foi executada |
| Propriedades do Usuário | Propriedades definidas pelo usuário da atividade |
| Erro do | Se a atividade falhou, o erro de execução |
| ID da execução | ID da execução de atividade |

![Exibição de lista para monitoramento de execuções de atividade](media/monitor-visually/activity-runs.png)

### <a name="promote-user-properties-to-monitor"></a>Promover as propriedades de usuário para monitorá-las

Promova qualquer propriedade de atividade de pipeline como uma propriedade de usuário para que ela se torne uma entidade que você monitora. Por exemplo, você pode promover as propriedades de **origem** e **destino** da atividade de cópia em seu pipeline como propriedades do usuário. Selecione **gerar automaticamente** para gerar as propriedades do usuário de **origem** e de **destino** para uma atividade de cópia.

![Criar propriedades do usuário](media/monitor-visually/monitor-user-properties-image1.png)

> [!NOTE]
> Você só pode promover até cinco Propriedades de atividade de pipeline como propriedades do usuário.

Depois de criar as propriedades do usuário, você pode monitorá-las nos modos de exibição da lista de monitoramento. Se a origem da atividade de cópia for um nome de tabela, você poderá monitorar o nome da tabela de origem como uma coluna no modo de exibição de lista para execuções de atividade.

![Lista de execuções de atividade sem propriedades de usuário](media/monitor-visually/monitor-user-properties-image2.png)

![Adicionar colunas para propriedades de usuário à lista de execuções de atividade](media/monitor-visually/monitor-user-properties-image3.png)

![Lista de execuções de atividade com colunas de propriedades de usuário](media/monitor-visually/monitor-user-properties-image4.png)

## <a name="configure-the-list-view"></a>Configurar a exibição de lista

### <a name="order-and-filter"></a>Ordenar e filtrar

Alterne se as execuções de pipeline serão em ordem decrescente ou crescente de acordo com a hora de início da execução. Filtrar execuções de pipeline usando as seguintes colunas:

| **Nome da coluna** | **Descrição** |
| --- | --- |
| Nome do Pipeline | Filtre pelo nome do pipeline. |
| Início da Execução |  Determine o intervalo de tempo das execuções de pipeline exibidas. As opções incluem filtros rápidos para as **últimas 24 horas**, **semana passada**e **últimos 30 dias** ou para selecionar uma data e hora personalizadas. |
| Status da execução | O filtro é executado por status: **êxito**, **falha**, **enfileirado**, **cancelado**ou **em andamento**. |
| Anotações | Filtrar por marcas aplicadas a cada pipeline |
| Execuções | Filtrar se você deseja ver os pipelines do reexecutei |

![Opções de filtragem](media/monitor-visually/filter.png)

### <a name="add-or-remove-columns"></a>Adicionar ou remover colunas
Clique com o botão direito do mouse no cabeçalho da exibição de lista e escolha as colunas que você deseja exibir na exibição de lista.

![Opções para colunas](media/monitor-visually/columns.png)

### <a name="adjust-column-widths"></a>Ajustar as larguras de colunas
Aumente e diminua as larguras de coluna no modo de exibição de lista passando o mouse sobre o cabeçalho da coluna.

## <a name="rerun-activities-inside-a-pipeline"></a>Executar atividades novamente em um pipeline

Você pode executar novamente as atividades dentro de um pipeline. Selecione **Exibir execuções de atividade**e, em seguida, selecione a atividade no pipeline da qual ponto você deseja executar novamente o pipeline.

![Exibir execuções de atividade](media/monitor-visually/rerun-activities-image1.png)

![Selecionar uma execução de atividade](media/monitor-visually/rerun-activities-image2.png)

### <a name="rerun-from-failed-activity"></a>Executar novamente a partir da atividade com falha

Se uma atividade falhar, expirar ou for cancelada, você poderá executar novamente o pipeline a partir dessa atividade com falha selecionando **executar novamente a partir da atividade com falha**.

![Executar novamente a atividade com falha](media/monitor-visually/rerun-failed-activity.png)

### <a name="view-rerun-history"></a>Exibir o histórico de novas execuções

Exiba o histórico de novas execuções para todas as execuções de pipeline na exibição de lista.

![Exibir histórico](media/monitor-visually/rerun-history-image1.png)

Exiba também o histórico de novas execuções para uma execução de pipeline específica.

![Exibir o histórico para uma execução de pipeline](media/monitor-visually/rerun-history-image2.png)

## <a name="gantt-views"></a>Exibições de Gantt

Use exibições de Gantt para visualizar rapidamente os pipelines e as execuções de atividade.

![Exemplo de um gráfico de Gantt](media/monitor-visually/gantt1.png)

Você pode examinar o modo de exibição de Gantt por pipeline ou por anotações/marcas que você criou em seus pipelines.

![Anotações do gráfico de Gantt](media/monitor-visually/gantt2.png)

O comprimento da barra informa a duração do pipeline. Você também pode selecionar a barra para ver mais detalhes.

![Duração do gráfico de Gantt](media/monitor-visually/gantt3.png)

## <a name="guided-tours"></a>Passeios guiados
Selecione o ícone de **informações** no canto inferior esquerdo. Em seguida, selecione **viagens guiadas** para obter instruções passo a passo sobre como monitorar suas execuções de pipeline e de atividade.

![Passeios guiados](media/monitor-visually/guided-tours.png)

## <a name="alerts"></a>Alertas

Você pode gerar alertas em métricas com suporte no Data Factory. Selecione **monitorar** > **alertas & métricas** na página monitoramento de data Factory para começar.

![Página monitor do data Factory](media/monitor-visually/alerts01.png)

Para uma introdução de sete minutos e uma demonstração desse recurso, assista ao vídeo a seguir:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Monitor-your-Azure-Data-Factory-pipelines-proactively-with-alerts/player]

### <a name="create-alerts"></a>Criar alertas

1.  Selecione **nova regra de alerta** para criar um novo alerta.

    ![Botão nova regra de alerta](media/monitor-visually/alerts02.png)

1.  Especifique o nome da regra e selecione a severidade do alerta.

    ![Caixas para nome da regra e severidade](media/monitor-visually/alerts03.png)

1.  Selecione os critérios de alerta.

    ![Caixa para critérios de destino](media/monitor-visually/alerts04.png)

    ![Lista de critérios](media/monitor-visually/alerts05.png)

1.  Configure a lógica de alerta. Crie um alerta para a métrica selecionada para todos os pipelines e atividades correspondentes. Você também pode selecionar um tipo específico de atividade, nome da atividade, nome do pipeline ou tipo de falha.

    ![Opções para configurar a lógica de alerta](media/monitor-visually/alerts06.png)

1.  Configure notificações por email, SMS, push e voz para o alerta. Crie um grupo de ações ou escolha um existente para as notificações de alerta.

    ![Opções para configurar notificações](media/monitor-visually/alerts07.png)

    ![Opções para adicionar uma notificação](media/monitor-visually/alerts08.png)

1.  Criar a regra de alerta.

    ![Opções para criar uma regra de alerta](media/monitor-visually/alerts09.png)

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre como monitorar e gerenciar pipelines, confira o artigo [monitorar e gerenciar pipelines programaticamente](https://docs.microsoft.com/azure/data-factory/monitor-programmatically) .
