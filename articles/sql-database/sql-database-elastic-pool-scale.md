---
title: Dimensionar recursos do pool elástico
description: Esta página descreve os recursos de escala para pools elásticos no Banco de Dados SQL do Azure.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
ms.date: 3/14/2019
ms.openlocfilehash: daca108cfc8bb2e5b2a068170a4a0244c72c9592
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77462591"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Dimensionar os recursos de pool elástico no banco de dados SQL do Azure

Este artigo descreve como dimensionar os recursos de computação e armazenamento disponíveis para pools elásticos e bancos de dados em pool no Banco de Dados SQL do Azure.

## <a name="change-compute-resources-vcores-or-dtus"></a>Alterar recursos de computação (vCores ou DTUs)

Depois de escolher inicialmente o número de vCores ou eDTUs, você pode dimensionar um pool elástico para cima ou para baixo dinamicamente com base na experiência real usando o [portal do Azure](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), o [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), o [CLI do Azure](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update)ou a [API REST](https://docs.microsoft.com/rest/api/sql/elasticpools/update).

### <a name="impact-of-changing-service-tier-or-rescaling-compute-size"></a>Impacto da alteração da camada de serviço ou redimensionamento do tamanho da computação

Alterar a camada de serviço ou o tamanho de computação de um pool elástico segue um padrão semelhante para bancos de dados individuais e, principalmente, envolve o serviço executando as seguintes etapas:

1. Criar nova instância de computação para o pool elástico  

    Uma nova instância de computação para o pool elástico é criada com a camada de serviço e o tamanho de computação solicitados. Para algumas combinações de camada de serviço e alterações de tamanho de computação, uma réplica de cada banco de dados deve ser criada na nova instância de computação que envolve a cópia de dados e pode influenciar de forma forte a latência geral. Independentemente disso, os bancos de dados permanecem online durante essa etapa, e as conexões continuam a ser direcionadas para os bancos de dados na instância de computação original.

2. Alternar o roteamento de conexões para a nova instância de computação

    As conexões existentes com os bancos de dados na instância de computação original são descartadas. Todas as novas conexões são estabelecidas com os bancos de dados na nova instância de computação. Para algumas combinações de camada de serviço e alterações de tamanho de computação, os arquivos de banco de dados são desanexados e reanexados durante o comutador.  Independentemente da opção, o switch pode resultar em uma breve interrupção do serviço quando os bancos de dados estiverem indisponíveis geralmente por menos de 30 segundos e geralmente por apenas alguns segundos. Se houver transações de longa execução em execução quando as conexões forem descartadas, a duração dessa etapa poderá levar mais tempo para recuperar as transações anuladas. A [recuperação de banco de dados acelerada](sql-database-accelerated-database-recovery.md) pode reduzir o impacto da anulação de transações de longa execução.

> [!IMPORTANT]
> Nenhum dado é perdido durante qualquer etapa no fluxo de trabalho.

### <a name="latency-of-changing-service-tier-or-rescaling-compute-size"></a>Latência da alteração da camada de serviço ou redimensionamento do tamanho da computação

A latência estimada para alterar a camada de serviço ou redimensionar o tamanho de computação de um único banco de dados ou pool elástico é parametrizada da seguinte maneira:

|Camada de serviço|Banco de dados individual básico,</br>Padrão (S0-S1)|Pool elástico básico,</br>Standard (S2-S12), </br>Em hiperescala </br>Uso Geral banco de dados individual ou pool elástico|Banco de dados único ou pool elástico Premium ou Comercialmente Crítico|
|:---|:---|:---|:---|
|**Banco de dados individual</br> básico, Standard (S0-S1)**|&bull;&nbsp;Latência de tempo constante independente do espaço usado</br>&bull;&nbsp;Normalmente, menos de 5 minutos|&bull;&nbsp;Latência proporcional ao espaço de banco de dados usado devido à cópia do dado</br>&bull;&nbsp;Normalmente, menos de 1 minuto por GB de espaço usado|&bull;&nbsp;Latência proporcional ao espaço de banco de dados usado devido à cópia do dado</br>&bull;&nbsp;Normalmente, menos de 1 minuto por GB de espaço usado|
|**Pool elástico básico, </br>Standard (S2-S12), </br>hiperescala, </br>uso geral banco de dados individual ou pool elástico**|&bull;&nbsp;Latência proporcional ao espaço de banco de dados usado devido à cópia do dado</br>&bull;&nbsp;Normalmente, menos de 1 minuto por GB de espaço usado|&bull;&nbsp;Latência de tempo constante independente do espaço usado</br>&bull;&nbsp;Normalmente, menos de 5 minutos|&bull;&nbsp;Latência proporcional ao espaço de banco de dados usado devido à cópia do dado</br>&bull;&nbsp;Normalmente, menos de 1 minuto por GB de espaço usado|
|**Banco de dados único ou pool elástico Premium ou Comercialmente Crítico**|&bull;&nbsp;Latência proporcional ao espaço de banco de dados usado devido à cópia do dado</br>&bull;&nbsp;Normalmente, menos de 1 minuto por GB de espaço usado|&bull;&nbsp;Latência proporcional ao espaço de banco de dados usado devido à cópia do dado</br>&bull;&nbsp;Normalmente, menos de 1 minuto por GB de espaço usado|&bull;&nbsp;Latência proporcional ao espaço de banco de dados usado devido à cópia do dado</br>&bull;&nbsp;Normalmente, menos de 1 minuto por GB de espaço usado|

> [!NOTE]
>
> - No caso de alterar a camada de serviço ou redimensionar a computação para um pool elástico, a soma do espaço usado em todos os bancos de dados no pool deve ser usada para calcular a estimativa.
> - No caso de mover um banco de dados para/de um pool elástico, somente o espaço usado pelo banco de dados afeta a latência, não o espaço usado pelo pool elástico.
>
> [!TIP]
> Para monitorar as operações em andamento, consulte: [gerenciar operações usando a API REST do SQL](https://docs.microsoft.com/rest/api/sql/operations/list), [gerenciar operações usando a CLI](/cli/azure/sql/db/op), [monitorar operações usando o T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) e estes dois comandos do PowerShell: [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) e [Stop-AzSqlDatabaseActivity](/powershell/module/az.sql/stop-azsqldatabaseactivity).

### <a name="additional-considerations-when-changing-service-tier-or-rescaling-compute-size"></a>Considerações adicionais ao alterar a camada de serviço ou redimensionar o tamanho da computação

- Quando downsizing vCores ou eDTUs para um pool elástico, o espaço usado do pool deve ser menor do que o tamanho máximo permitido da camada de serviço de destino e do pool eDTUs.
- Ao redimensionar eDTUs de um pool Elástico, um custo de armazenamento extra será aplicado se (1) o tamanho máximo de armazenamento do pool é compatível com o pool de destino, e (2) o tamanho máximo do armazenamento excede a quantidade de armazenamento incluída do pool de destino. Por exemplo, se um pool Standard de 100 eDTU com um tamanho máximo de 100 GB for reduzido para um pool Standard de 50 eDTU, um custo de armazenamento extra será aplicado, pois o pool de destino dá suporte a um tamanho máximo de 100 GB, e a quantidade de armazenamento incluído é somente de 50 GB. Assim, a quantidade de armazenamento extra será 100 GB – 50 GB = 50 GB. Para obter o preço do armazenamento extra, confira [Preços do Banco de Dados SQL](https://azure.microsoft.com/pricing/details/sql-database/). Se a quantidade real de espaço usado for menor do que a quantidade de armazenamento incluído, esse custo extra poderá ser evitado por meio da redução do tamanho máximo do banco de dados para a quantidade incluída.

### <a name="billing-during-rescaling"></a>Cobrança durante o redimensionamento

Você será cobrado pelas horas em que um banco de dados existir usando a camada de serviço mais alta mais o tamanho da computação aplicado durante essas horas, independentemente do uso ou se o banco de dados ficou ativo por menos de uma hora. Por exemplo, se você criar um banco de dados individual e o excluir depois de cinco minutos, sua fatura apresentará uma cobrança referente a uma hora de banco de dados.

## <a name="change-elastic-pool-storage-size"></a>Alterar o tamanho do armazenamento do pool elástico

> [!IMPORTANT]
> Em algumas circunstâncias, talvez seja necessário reduzir um banco de dados para recuperar o espaço não utilizado. Para obter mais informações, consulte [gerenciar o espaço de arquivo no banco de dados SQL do Azure](sql-database-file-space-management.md).

### <a name="vcore-based-purchasing-model"></a>Modelo de compra baseado em vCore

- O armazenamento pode ser provisionado até o limite de tamanho máximo:

  - Para armazenamento nas camadas de serviço standard ou de uso geral, aumente ou diminua o tamanho em incrementos de 10 GB
  - Para armazenamento nas camadas de serviço críticas premium ou empresariais, aumente ou diminua o tamanho em incrementos de 250 GB
- O armazenamento de um pool elástico pode ser provisionado aumentando ou diminuindo seu tamanho máximo.
- O preço do armazenamento para um pool elástico é a quantidade de armazenamento multiplicada pelo preço unitário do armazenamento da camada de serviço. Para obter detalhes sobre o preço de armazenamento extra, confira [Preços do Banco de Dados SQL](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Em algumas circunstâncias, talvez seja necessário reduzir um banco de dados para recuperar o espaço não utilizado. Para obter mais informações, consulte [gerenciar o espaço de arquivo no banco de dados SQL do Azure](sql-database-file-space-management.md).

### <a name="dtu-based-purchasing-model"></a>Modelo de compra baseado em DTU

- O preço de eDTU para um pool elástico inclui uma determinada quantidade de armazenamento sem custo adicional. O armazenamento extra além da quantidade incluída pode ser provisionado mediante um custo adicional até o limite máximo de tamanho, em incrementos de 250 GB até 1 TB e, em seguida, em incrementos de 256 GB além de 1 TB. Para conhecer as quantidades de armazenamento incluídos e os limites de tamanho máximos, confira [Pool elástico: tamanhos de armazenamento e de computação](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
- É possível provisionar o armazenamento extra para um pool elástico aumentando seu tamanho máximo usando o [Portal do Azure](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), a [CLI do Azure](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update) ou a [API REST](https://docs.microsoft.com/rest/api/sql/elasticpools/update).
- O preço do armazenamento extra para um pool elástico é a quantidade de armazenamento extra multiplicada pelo preço unitário do armazenamento extra da camada de serviço. Para obter detalhes sobre o preço de armazenamento extra, confira [Preços do Banco de Dados SQL](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Em algumas circunstâncias, talvez seja necessário reduzir um banco de dados para recuperar o espaço não utilizado. Para obter mais informações, consulte [gerenciar o espaço de arquivo no banco de dados SQL do Azure](sql-database-file-space-management.md).

## <a name="next-steps"></a>Próximas etapas

Para saber os limites de recurso global, confira [Limites de recurso baseado em vCore do banco de dados SQL – pools elásticos](sql-database-vcore-resource-limits-elastic-pools.md) e [Limites de DTU de banco de dados do SQL com base em recurso – pools elásticos](sql-database-dtu-resource-limits-elastic-pools.md).
