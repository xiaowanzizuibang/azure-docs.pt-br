---
title: 'Tutorial: replicação geográfica & failover no portal'
description: Configure a replicação geográfica para um banco de dados individual ou em pool no banco de dados SQL do Azure usando o portal do Azure e inicie o failover.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 02/13/2019
ms.openlocfilehash: 59616fb217b28a8c47d9a5d13e2f4c1b9a8f6bb3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81605217"
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-the-azure-portal-and-initiate-failover"></a>Configurar a replicação geográfica ativa para o Banco de Dados SQL do Azure usando o Portal do Azure e inicializar o failover

Este artigo mostra como configurar a [replicação geográfica ativa para bancos de dados individuais e em pool](sql-database-active-geo-replication.md#active-geo-replication-terminology-and-capabilities) no Banco de Dados SQL do Azure usando o [portal do Azure](https://portal.azure.com) e como inicializar o failover.

Para obter informações sobre grupos de failover automático com bancos de dados individuais e em pool, confira [Práticas recomendadas de como usar grupos de failover com bancos de dados individuais e em pool](sql-database-auto-failover-group.md#best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools). Para obter informações sobre grupos de failover automático com instâncias gerenciadas, consulte [práticas recomendadas de uso de grupos de failover com instâncias gerenciadas](sql-database-auto-failover-group.md#best-practices-of-using-failover-groups-with-managed-instances).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a replicação geográfica ativa usando o Portal do Azure, você precisa do seguinte recurso:

* Um Banco de Dados SQL do Azure: o banco de dados primário que você deseja replicar para uma região geográfica diferente.

> [!Note]
> Ao usar o portal do Azure, você só pode criar um banco de dados secundário na mesma assinatura que o primário. Caso o banco de dados secundário deva estar em uma assinatura diferente, use [Criar banco de dados REST API](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) ou [API de Transact-SQL de ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql).

## <a name="add-a-secondary-database"></a>Adicionar um banco de dados secundário

As etapas a seguir criam um novo banco de dados secundário em uma parceria de replicação geográfica.  

Para adicionar um banco de dados secundário, você deve ser o proprietário ou coproprietário da assinatura.

O banco de dados secundário tem o mesmo nome do banco de dados primário e, por padrão, tem a mesma camada de serviço e tamanho da computação. O banco de dados secundário pode ser um banco de dados individual ou um banco de dados em pool. Para obter mais informações, consulte [Modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md) e [Modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md).
Depois que o banco de dados secundário for criado e propagado, os dados começarão a serem replicados desde o banco de dados primário até o novo banco de dados secundário.

> [!NOTE]
> O comando falhará se o banco de dados do parceiro já existir (por exemplo, como resultado do término de uma relação de replicação geográfica anterior).

1. Na [portal do Azure](https://portal.azure.com), navegue até o banco de dados que você deseja configurar para a replicação geográfica.
2. Na página de banco de dados SQL, selecione **replicação geográfica** e, em seguida, selecione a região para criar o banco de dados secundário. É possível selecionar qualquer região além da região que hospeda o banco de dados primário, mas recomendamos a [região emparelhada](../best-practices-availability-paired-regions.md).

    ![Configurar a replicação geográfica](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. Selecione ou configure o servidor e o tipo de preço do banco de dados secundário.

    ![Configurar secundário](./media/sql-database-geo-replication-portal/create-secondary.png)
4. Opcionalmente, você pode adicionar um banco de dados secundário a um pool elástico. Para criar o banco de dados secundário em um pool, clique em **pool elástico** e selecione um pool no servidor de destino. Um pool já deve existir no servidor de destino. Esse fluxo de trabalho não cria um pool.
5. Clique em **Criar** para adicionar o secundário.
6. O banco de dados secundário é criado e começa o processo de propagação.

    ![Configurar secundário](./media/sql-database-geo-replication-portal/seeding0.png)
7. Quando o processo de propagação for concluído, o banco de dados secundário exibirá seu status.

    ![Propagação concluída](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a>Iniciar um failover

O banco de dados secundário pode ser alternado para se tornar primário.  

1. Na [portal do Azure](https://portal.azure.com), navegue até o banco de dados primário na parceria de replicação geográfica.
2. Na folha banco de dados SQL, selecione **todas as configurações** > **replicação geográfica**.
3. Na lista **secundários** , selecione o banco de dados que você deseja tornar o novo primário e clique em **failover forçado**.

    ![failover](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Clique em **Sim** para iniciar o failover.

O comando mudará imediatamente o banco de dados secundário para a função primária. Esse processo normalmente deve ser concluído dentro de 30 segundos ou menos.

Há um breve período durante o qual os bancos de dados não estão disponíveis (na ordem de 0 a 25 segundos) enquanto as funções são alternadas. Se o banco de dados primário tiver vários bancos de dados secundários, o comando reconfigurará automaticamente os outros secundários para se conectarem ao novo primário. A operação inteira deve levar menos de um minuto para ser concluída em circunstâncias normais.

> [!NOTE]
> Este comando destina-se à recuperação rápida do banco de dados em caso de interrupção. Ele dispara o failover sem sincronização de dados (failover forçado).  Se o primário estiver online e realizando transações quando o comando for emitido, alguma perda de dados poderá ocorrer.

## <a name="remove-secondary-database"></a>Remover um banco de dados secundário

Essa operação termina permanentemente a replicação para o banco de dados secundário e altera a função do secundário para um banco de dados de leitura/gravação normal. Se a conectividade com o banco de dados secundário for desfeita, o comando terá êxito, mas o secundário só se tornará de leitura/gravação quando a conectividade for restaurada.  

1. Na [portal do Azure](https://portal.azure.com), navegue até o banco de dados primário na parceria de replicação geográfica.
2. Na página banco de dados SQL, selecione **replicação geográfica**.
3. Na lista **secundários** , selecione o banco de dados que você deseja remover da parceria de replicação geográfica.
4. Clique em **Parar Replicação**.

    ![Remover secundário](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. Uma janela de confirmação é aberta. Clique em **Sim** para remover o banco de dados da parceria de replicação geográfica. (Defina-o como um banco de dados de leitura/gravação que não faz parte de nenhuma replicação.)

## <a name="next-steps"></a>Próximas etapas

* Para saber mais sobre a replicação geográfica ativa, consulte [replicação geográfica ativa](sql-database-active-geo-replication.md).
* Para saber mais sobre grupos de failover automático, confira [Grupos de failover automático](sql-database-auto-failover-group.md)
* Para obter uma visão geral e cenários de continuidade de negócios, consulte [visão geral da continuidade de negócios](sql-database-business-continuity.md)
