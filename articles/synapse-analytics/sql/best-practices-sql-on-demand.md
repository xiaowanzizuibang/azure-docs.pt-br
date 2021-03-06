---
title: Práticas recomendadas para SQL sob demanda (versão prévia) no Azure Synapse Analytics
description: Recomendações e práticas recomendadas que você deve saber ao trabalhar com o SQL sob demanda (versão prévia).
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 1d4203141973c10fe7673f6ab9dedbc3bfdc8999
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81429064"
---
# <a name="best-practices-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Práticas recomendadas para SQL sob demanda (versão prévia) no Azure Synapse Analytics

Neste artigo, você encontrará uma coleção de práticas recomendadas para usar o SQL sob demanda (versão prévia). O SQL sob demanda é um recurso adicional no Azure Synapse Analytics.

## <a name="general-considerations"></a>Considerações gerais

O SQL sob demanda permite consultar arquivos em suas contas de armazenamento do Azure. Ele não tem armazenamento local nem recursos de ingestão. Assim, todos os arquivos que a consulta visa são externos ao SQL sob demanda. Tudo relacionado à leitura de arquivos do armazenamento pode ter um impacto no desempenho da consulta.

## <a name="colocate-azure-storage-account-and-sql-on-demand"></a>Colocar a conta de armazenamento do Azure e o SQL sob demanda

Para minimizar a latência, coloque a conta de armazenamento do Azure e o ponto de extremidade sob demanda do SQL. As contas de armazenamento e os pontos de extremidade provisionados durante a criação do espaço de trabalho estão localizados na mesma região.

Para obter um desempenho ideal, se você acessar outras contas de armazenamento com o SQL sob demanda, verifique se elas estão na mesma região. Se eles não estiverem na mesma região, haverá uma latência maior para a transferência de rede dos dados entre as regiões do ponto de extremidade e remoto.

## <a name="azure-storage-throttling"></a>Limitação de armazenamento do Azure

Vários aplicativos e serviços podem acessar sua conta de armazenamento. A limitação de armazenamento ocorre quando o IOPS combinado ou a taxa de transferência gerada por aplicativos, serviços e carga de trabalho SQL sob demanda excede os limites da conta de armazenamento. Como resultado, você enfrentará um efeito negativo significativo no desempenho da consulta.

Depois que a limitação é detectada, o SQL sob demanda tem manipulação interna desse cenário. O SQL sob demanda fará solicitações de armazenamento em um ritmo mais lento até que a limitação seja resolvida.

> [!TIP]
> Para a execução de consulta ideal, você não deve enfatizar a conta de armazenamento com outras cargas de trabalho durante a execução da consulta.

## <a name="prepare-files-for-querying"></a>Preparar arquivos para consulta

Se possível, você pode preparar arquivos para melhorar o desempenho:

- Converta CSV em parquet-parquet é o formato de coluna. Como ele está compactado, seus tamanhos de arquivo são menores do que arquivos CSV com os mesmos dados. O SQL sob demanda precisará de menos tempo e solicitações de armazenamento para lê-lo.
- Se uma consulta tiver como alvo um único arquivo grande, você se beneficiará de dividi-lo em vários arquivos menores.
- Tente manter o tamanho do arquivo CSV abaixo de 10 GB.
- É melhor ter arquivos igualmente dimensionados para um único caminho OPENROWSET ou um local de tabela externa.
- Particione seus dados armazenando partições em diferentes pastas ou nomes de arquivo-marque as [funções filename e filePath para direcionar partições específicas](#use-fileinfo-and-filepath-functions-to-target-specific-partitions).

## <a name="use-fileinfo-and-filepath-functions-to-target-specific-partitions"></a>Usar funções FileInfo e filePath para direcionar partições específicas

Os dados geralmente são organizados em partições. Você pode instruir o SQL sob demanda para consultar arquivos e pastas particulares. Essa função reduzirá o número de arquivos e a quantidade de dados que a consulta precisa ler e processar. Um bônus adicional é que você obterá um melhor desempenho.

Para obter mais informações, verifique as funções [filename](develop-storage-files-overview.md#filename-function) e [FilePath](develop-storage-files-overview.md#filepath-function) e exemplos sobre como [consultar arquivos específicos](query-specific-files.md).

Se os dados armazenados não estiverem particionados, considere particioná-los para que você possa usar essas funções para otimizar as consultas direcionadas a esses arquivos. Ao [consultar tabelas do Spark particionadas](develop-storage-files-spark-tables.md) do SQL sob demanda, a consulta será automaticamente direcionada somente aos arquivos necessários.

## <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Use o CETAS para aprimorar o desempenho e as junções de consulta

O [CETAS](develop-tables-cetas.md) é um dos recursos mais importantes disponíveis no SQL sob demanda. CETAS é uma operação paralela que cria metadados de tabela externa e exporta os resultados da consulta SELECT para um conjunto de arquivos em sua conta de armazenamento.

Você pode usar CETAS para armazenar partes usadas com frequência de consultas, como tabelas de referência Unidas, em um novo conjunto de arquivos. Em seguida, você pode unir a esta única tabela externa em vez de repetir junções comuns em várias consultas.

Como CETAS gera arquivos parquet, as estatísticas serão criadas automaticamente quando a primeira consulta for direcionada a essa tabela externa, resultando em um desempenho aprimorado.

## <a name="next-steps"></a>Próximas etapas

Examine o artigo de [solução de problemas](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) para problemas comuns e soluções. Se você estiver trabalhando com o pool do SQL em vez de SQL sob demanda, consulte o artigo [práticas recomendadas para o pool do SQL](best-practices-sql-pool.md) para obter diretrizes específicas.
