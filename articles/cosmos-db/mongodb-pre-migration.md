---
title: Etapas de pré-migração para migração de dados para a API do Azure Cosmos DB para MongoDB
description: Este documento fornece uma visão geral dos pré-requisitos para uma migração de dados do MongoDB para Cosmos DB.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 01/09/2020
ms.author: lbosq
ms.openlocfilehash: 9e867a544de8904274286cb68fc047f3f4b93e0d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82183306"
---
# <a name="pre-migration-steps-for-data-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>Etapas de pré-migração para migrações de dados do MongoDB para a API do Azure Cosmos DB para o MongoDB

Antes de migrar seus dados do MongoDB (local ou na nuvem) para a API de Azure Cosmos DB para MongoDB, você deve:

1. [Leia as principais considerações sobre como usar a API do Azure Cosmos DB para MongoDB](#considerations)
2. [Escolha uma opção para migrar seus dados](#options)
3. [Estimar a taxa de transferência necessária para suas cargas de trabalho](#estimate-throughput)
4. [Escolha uma chave de partição ideal para seus dados](#partitioning)
5. [Entender a política de indexação que você pode definir em seus dados](#indexing)

Se você já tiver concluído os pré-requisitos acima para a migração, poderá [migrar dados do MongoDB para a API do Azure Cosmos DB para MongoDB usando o serviço de migração de banco de dados do Azure](../dms/tutorial-mongodb-cosmos-db.md). Além disso, se você não tiver criado uma conta, poderá navegar por qualquer um dos [guias de início rápido](create-mongodb-dotnet.md) que mostram as etapas para criar uma conta.

## <a name="considerations-when-using-azure-cosmos-dbs-api-for-mongodb"></a><a id="considerations"></a>Considerações ao usar a API do Azure Cosmos DB para MongoDB

Veja a seguir características específicas sobre a API do Azure Cosmos DB para MongoDB:

- **Modelo de capacidade**: a capacidade do banco de dados no Azure Cosmos DB é baseada em um modelo baseado em taxa de transferência. Esse modelo é baseado em [unidades de solicitação por segundo](request-units.md), que é uma unidade que representa o número de operações de banco de dados que podem ser executadas em uma coleção por segundo. Essa capacidade pode ser alocada em [um nível de banco de dados ou de coleção](set-throughput.md)e pode ser provisionada em um modelo de alocação ou usando o [modelo de dimensionamento automático](provision-throughput-autoscale.md).

- **Unidades de solicitação**: cada operação de banco de dados tem um custo de RUs (unidades de solicitação) associadas em Azure Cosmos DB. Quando executado, isso é subtraído do nível de unidades de solicitação disponíveis em um determinado segundo. Se uma solicitação exigir mais RUs do que o RU/s alocado atualmente, há duas opções para resolver o problema-aumente a quantidade de RUs ou aguarde até que o próximo segundo inicie e repita a operação.

- **Capacidade elástica**: a capacidade de uma determinada coleção ou banco de dados pode ser alterada a qualquer momento. Isso permite que o banco de dados se adapte de forma elástica aos requisitos de taxa de transferência de sua carga de trabalho.

- **Fragmentação automática**: o Azure Cosmos DB fornece um sistema de particionamento automático que requer apenas um fragmento (ou uma chave de partição). O [mecanismo de particionamento automático](partition-data.md) é compartilhado entre todas as APIs de Azure Cosmos DB e permite dados contínuos e ao longo do dimensionamento por meio da distribuição horizontal.

## <a name="migration-options-for-azure-cosmos-dbs-api-for-mongodb"></a><a id="options"></a>Opções de migração para a API do Azure Cosmos DB para MongoDB

O serviço de migração de banco de dados do [Azure para a API do Azure Cosmos DB para MongoDB](../dms/tutorial-mongodb-cosmos-db.md) fornece um mecanismo que simplifica a migração do dado fornecendo uma plataforma de hospedagem totalmente gerenciada, opções de monitoramento de migração e manipulação de limitação automática. A lista completa de opções é a seguinte:

|**Tipo de migração**|**Solução**|**Considerações**|
|---------|---------|---------|
|Offline|[Ferramenta de migração de dados](https://docs.microsoft.com/azure/cosmos-db/import-data)|&bull;Fácil de configurar e dar suporte a várias fontes <br/>&bull;Não é adequado para grandes conjuntos de altos.|
|Offline|[Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-cosmos-db)|&bull;Fácil de configurar e dar suporte a várias fontes <br/>&bull;Usa a biblioteca de executores em massa Azure Cosmos DB <br/>&bull;Adequado para grandes conjuntos de altos <br/>&bull;A falta de ponto de verificação significa que qualquer problema durante o curso da migração exigiria uma reinicialização do processo de migração inteiro<br/>&bull;A falta de uma fila de mensagens mortas significa que alguns arquivos errados poderiam parar todo o processo de migração <br/>&bull;Precisa de código personalizado para aumentar a taxa de transferência de leitura para determinadas fontes de dados|
|Offline|[Ferramentas de Mongo existentes (mongodump, mongorestore, Studio3T)](https://azure.microsoft.com/resources/videos/using-mongodb-tools-with-azure-cosmos-db/)|&bull;Fácil de configurar e integrar <br/>&bull;Precisa de manipulação personalizada para limitação|
|Online|[Serviço de Migração de Banco de Dados do Azure](../dms/tutorial-mongodb-cosmos-db-online.md)|&bull;Serviço de migração totalmente gerenciado.<br/>&bull;Fornece soluções de hospedagem e monitoramento para a tarefa de migração. <br/>&bull;Adequado para grandes conjuntos de altos e cuida da replicação de alterações dinâmicas <br/>&bull;Funciona somente com outras fontes do MongoDB|


## <a name="estimate-the-throughput-need-for-your-workloads"></a><a id="estimate-throughput"></a>Estimar a necessidade de taxa de transferência para suas cargas de trabalho

Em Azure Cosmos DB, a taxa de transferência é provisionada antecipadamente e é medida em unidades de solicitação (RU) por segundo. Diferentemente das VMs ou servidores locais, o RUs é fácil de escalar e reduzir verticalmente a qualquer momento. Você pode alterar o número de RUs provisionadas instantaneamente. Para obter mais informações, confira [Unidades de solicitação no Azure Cosmos DB](request-units.md).

Você pode usar a [calculadora de capacidade de Azure Cosmos DB](https://cosmos.azure.com/capacitycalculator/) para determinar a quantidade de unidades de solicitação com base em sua configuração de conta de banco de dados, quantidade, tamanho do documento e leituras e gravações necessárias por segundo.

Veja a seguir os principais fatores que afetam o número de RUs necessário:
- **Tamanho do documento**: à medida que o tamanho de um item/documento aumenta, o número de RUs consumidas para ler ou gravar o item/documento também aumenta.

- **Contagem de propriedades do documento**: o número de RUs consumidas para criar ou atualizar um documento está relacionado ao número, à complexidade e ao comprimento de suas propriedades. Você pode reduzir o consumo de unidade de solicitação para operações de gravação [limitando o número de propriedades indexadas](mongodb-indexing.md).

- **Padrões de consulta**: a complexidade de uma consulta afeta quantas unidades de solicitação são consumidas pela consulta. 

A melhor maneira de entender o custo das consultas é usar dados de exemplo em Azure Cosmos DB [e executar consultas de exemplo do shell do MongoDB](connect-mongodb-account.md) usando o `getLastRequestStastistics` comando para obter o encargo da solicitação, o que produzirá o número de RUs consumidas:

`db.runCommand({getLastRequestStatistics: 1})`

Esse comando produzirá um documento JSON semelhante ao seguinte:

```{  "_t": "GetRequestStatisticsResponse",  "ok": 1,  "CommandName": "find",  "RequestCharge": 10.1,  "RequestDurationInMilliSeconds": 7.2}```

Você também pode usar as [configurações de diagnóstico](cosmosdb-monitor-resource-logs.md) para entender a frequência e os padrões das consultas executadas em relação à Azure Cosmos DB. Os resultados dos logs de diagnóstico podem ser enviados para uma conta de armazenamento, uma instância do EventHub ou [log Analytics do Azure](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).  

## <a name="choose-your-partition-key"></a><a id="partitioning"></a>Escolha sua chave de partição
O particionamento, também conhecido como fragmentação, é um ponto-chave de consideração antes da migração de dados. O Azure Cosmos DB usa particionamento totalmente gerenciado para aumentar a capacidade em um banco de dados para atender aos requisitos de armazenamento e taxa de transferência. Esse recurso não precisa de hospedagem ou configuração de servidores de roteamento.   

De maneira semelhante, o recurso de particionamento adiciona automaticamente a capacidade e reequilibra os dados de forma adequada. Para obter detalhes e recomendações sobre como escolher a chave de partição correta para seus dados, consulte o [artigo escolhendo uma chave de partição](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey). 

## <a name="index-your-data"></a><a id="indexing"></a>Indexar os dados
Por padrão, Azure Cosmos DB fornece indexação automática em todos os dados inseridos. Os recursos de indexação fornecidos pelo Azure Cosmos DB incluem a adição de índices compostos, índices exclusivos e índices TTL (vida útil). A interface de gerenciamento de índice é mapeada `createIndex()` para o comando. Saiba mais na [indexação na API do Azure Cosmos DB para MongoDB](mongodb-indexing.md).

O [serviço de migração de banco de dados do Azure](../dms/tutorial-mongodb-cosmos-db.md) migra coleções do MongoDB automaticamente com índices exclusivos. No entanto, os índices exclusivos devem ser criados antes da migração. Azure Cosmos DB não oferece suporte à criação de índices exclusivos, quando já existem dados em suas coleções. Para obter mais informações, veja [Chaves exclusivas no Azure Cosmos DB](unique-keys.md).

## <a name="next-steps"></a>Próximas etapas
* [Migre seus dados do MongoDB para Cosmos DB usando o serviço de migração de banco de dado.](../dms/tutorial-mongodb-cosmos-db.md) 
* [Taxa de transferência de provisionamento nos contêineres e bancos de dados do Azure Cosmos](set-throughput.md)
* [Particionamento no BD Cosmos do Azure](partition-data.md)
* [Distribuição global no Azure Cosmos DB](distribute-data-globally.md)
* [Indexação no Azure Cosmos DB](index-overview.md)
* [Unidades de solicitação no Azure Cosmos DB](request-units.md)
