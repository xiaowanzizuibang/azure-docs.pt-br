---
title: Copiar dados do DB2 usando o Azure Data Factory
description: Saiba como copiar dados do DB2 para armazenamentos de dados de coletor com suporte usando uma atividade de cópia em um pipeline do Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: jingwang
ms.openlocfilehash: 2c2071e4b2a3daa528c7d01f64e38247b063e6f1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81417427"
---
# <a name="copy-data-from-db2-by-using-azure-data-factory"></a>Copiar dados do DB2 usando o Azure Data Factory
> [!div class="op_single_selector" title1="Selecione a versão do serviço Data Factory que você está usando:"]
> * [Versão 1](v1/data-factory-onprem-db2-connector.md)
> * [Versão atual](connector-db2.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Este artigo descreve como usar a atividade de cópia no Azure Data Factory para copiar dados de um banco de dados DB2. Ele amplia o artigo [Visão geral da atividade de cópia](copy-activity-overview.md) que apresenta uma visão geral da atividade de cópia.

## <a name="supported-capabilities"></a>Funcionalidades com suporte

Este conector de banco de dados DB2 tem suporte para as seguintes atividades:

- [Atividade de cópia](copy-activity-overview.md) com [matriz de coletor/origem com suporte](copy-activity-overview.md)
- [Atividade de pesquisa](control-flow-lookup-activity.md)

Você pode copiar dados de um banco de dados DB2 para qualquer armazenamento de dados de coletor com suporte. Para obter uma lista de armazenamentos de dados com suporte como origens/coletores da atividade de cópia, confira a tabela [Armazenamentos de dados com suporte](copy-activity-overview.md#supported-data-stores-and-formats).

Especificamente, este conector DB2 dá suporte às plataformas e versões do IBM DB2 a seguir com as versões 9, 10 e 11 do SQL Access Manager (SQLAM) da Distributed Relational Database Architecture (DRDA):

* IBM DB2 para z/OS 12,1
* IBM DB2 para z/OS 11.1
* IBM DB2 para z/OS 10.1
* IBM DB2 para i 7.3
* IBM DB2 para i 7.2
* IBM DB2 para i 7.1
* IBM DB2 para LUW 11
* IBM DB2 para LUW 10.5
* IBM DB2 para LUW 10.1

>[!TIP]
>O conector do DB2 é criado com base em Provedor Microsoft OLE DB para DB2. Para solucionar problemas de erros do conector do DB2, consulte [provedor de dados códigos de erro](https://docs.microsoft.com/host-integration-server/db2oledbv/data-provider-error-codes#drda-protocol-errors).

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

O Integration Runtime fornece um driver do DB2 interno, portanto, não será necessário instalar manualmente nenhum driver ao copiar dados do DB2.

## <a name="getting-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As seções a seguir fornecem detalhes sobre as propriedades usadas para definir entidades do Data Factory específicas ao conector do DB2.

## <a name="linked-service-properties"></a>Propriedades do serviço vinculado

As propriedades a seguir têm suporte para o serviço vinculado do DB2:

| Propriedade | Descrição | Obrigatório |
|:--- |:--- |:--- |
| type | A propriedade type deve ser definida como: **Db2** | Sim |
| Servidor |Nome do servidor DB2. Você pode especificar o número da porta após o nome do servidor delimitado por dois pontos, por exemplo, `server:port`. |Sim |
| Banco de Dados |Nome do banco de dados DB2. |Sim |
| authenticationType |Tipo de autenticação usado para se conectar ao banco de dados DB2.<br/>O valor permitido é: **Básica**. |Sim |
| username |Especifica o nome de usuário para se conectar ao banco de dados DB2. |Sim |
| password |Especifique a senha da conta de usuário que você especificou para o nome de usuário. Marque esse campo como SecureString para armazená-lo com segurança no Data Factory ou [referencie um segredo armazenado no Cofre de Chaves do Azure](store-credentials-in-key-vault.md). |Sim |
| pacotecollection | Especifique em onde os pacotes necessários são criados automaticamente pelo ADF ao consultar o banco de dados. | Não |
| certificateCommonName | Ao usar o protocolo SSL (SSL) ou a criptografia TLS, você deve inserir um valor para o nome comum do certificado. | Não |
| connectVia | O [Integration Runtime](concepts-integration-runtime.md) a ser usado para se conectar ao armazenamento de dados. Saiba mais na seção de [pré-requisitos](#prerequisites) . Se não for especificado, ele usa o Integration Runtime padrão do Azure. |Não |

> [!TIP]
> Se você receber uma mensagem de erro afirmando `The package corresponding to an SQL statement execution request was not found. SQLSTATE=51002 SQLCODE=-805`, o motivo é que um pacote necessário não é criado para o usuário. Por padrão, o ADF tentará criar um pacote na coleção denominada como o usuário que você usou para se conectar ao DB2. Especifique a propriedade de coleção de pacote para indicar sob onde você deseja que o ADF crie os pacotes necessários ao consultar o banco de dados.

**Exemplo:**

```json
{
    "name": "Db2LinkedService",
    "properties": {
        "type": "Db2",
        "typeProperties": {
            "server": "<servername:port>",
            "database": "<dbname>",
            "authenticationType": "Basic",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para obter uma lista completa das seções e propriedades disponíveis para definir conjuntos de os, consulte o artigo [conjuntos de valores](concepts-datasets-linked-services.md) . Esta seção fornece uma lista das propriedades com suporte pelo conjunto de dados do DB2.

Para copiar dados do DB2, há suporte para as seguintes propriedades:

| Propriedade | Descrição | Obrigatório |
|:--- |:--- |:--- |
| type | A propriedade Type do conjunto de conjuntos deve ser definida como: **Db2Table** | Sim |
| esquema | Nome do esquema. |Não (se "query" na fonte da atividade for especificada)  |
| tabela | Nome da tabela. |Não (se "query" na fonte da atividade for especificada)  |
| tableName | Nome da tabela com esquema. Essa propriedade tem suporte para compatibilidade com versões anteriores. Use `schema` e `table` para uma nova carga de trabalho. | Não (se "query" na fonte da atividade for especificada) |

**Exemplo**

```json
{
    "name": "DB2Dataset",
    "properties":
    {
        "type": "Db2Table",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<DB2 linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Se você estivesse usando `RelationalTable` dataset tipado, ele ainda tem suporte como está, enquanto você é sugerido para usar o novo no futuro.

## <a name="copy-activity-properties"></a>Propriedades da atividade de cópia

Para obter uma lista completa das seções e propriedades disponíveis para definir atividades, confia o artigo [Pipelines](concepts-pipelines-activities.md). Esta seção fornece uma lista das propriedades com suporte pela fonte DB2.

### <a name="db2-as-source"></a>DB2 como fonte

Para copiar dados do DB2, há suporte para as seguintes propriedades na seção **origem** da atividade de cópia:

| Propriedade | Descrição | Obrigatório |
|:--- |:--- |:--- |
| type | A propriedade Type da fonte da atividade de cópia deve ser definida como: **Db2Source** | Sim |
| Consulta | Utiliza a consulta SQL personalizada para ler os dados. Por exemplo: `"query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""`. | Não (se "tableName" no conjunto de dados for especificado) |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromDB2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<DB2 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "Db2Source",
                "query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Se você estiver usando `RelationalSource` a fonte digitada, ainda haverá suporte como está, enquanto você é sugerido para usar a nova no futuro.

## <a name="data-type-mapping-for-db2"></a>Mapeamento de tipo de dados para o DB2

Ao copiar dados do DB2, os seguintes mapeamentos são usados de tipos de dados do DB2 para tipos de dados provisórios do Azure Data Factory. Consulte [Mapeamentos de tipo de dados e esquema](copy-activity-schema-and-type-mapping.md) para saber mais sobre como a atividade de cópia mapeia o tipo de dados e esquema de origem para o coletor.

| Tipo do Banco de Dados DB2 | Tipo de dados provisório do Data Factory |
|:--- |:--- |
| BigInt |Int64 |
| Binário |Byte[] |
| Blob |Byte[] |
| Char |Cadeia de caracteres |
| Clob |Cadeia de caracteres |
| Data |Datetime |
| DB2DynArray |Cadeia de caracteres |
| DbClob |Cadeia de caracteres |
| Decimal |Decimal |
| DecimalFloat |Decimal |
| Double |Double |
| Float |Double |
| Graphic |Cadeia de caracteres |
| Integer |Int32 |
| LongVarBinary |Byte[] |
| LongVarChar |Cadeia de caracteres |
| LongVarGraphic |Cadeia de caracteres |
| Numérico |Decimal |
| Real |Single |
| SmallInt |Int16 |
| Hora |TimeSpan |
| Timestamp |Datetime |
| VarBinary |Byte[] |
| VarChar |Cadeia de caracteres |
| VarGraphic |Cadeia de caracteres |
| Xml |Byte[] |

## <a name="lookup-activity-properties"></a>Propriedades da atividade de pesquisa

Para obter detalhes sobre as propriedades, verifique a [atividade de pesquisa](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Próximas etapas
Para obter uma lista de armazenamentos de dados com suporte como origens e coletores pela atividade de cópia no Azure Data Factory, consulte [Armazenamentos de dados com suporte](copy-activity-overview.md#supported-data-stores-and-formats).
