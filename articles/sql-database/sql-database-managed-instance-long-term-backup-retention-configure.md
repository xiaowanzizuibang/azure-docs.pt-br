---
title: 'Instância gerenciada: retenção de backup de longo prazo (PowerShell)'
description: Saiba como armazenar e restaurar backups automatizados em contêineres de armazenamento de BLOBs do Azure separados para uma instância gerenciada do banco de dados SQL do Azure usando o PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 04/29/2020
ms.openlocfilehash: fba1dcdb8c4e5cfaa8071ae58ad75432b526c887
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82509696"
---
# <a name="manage-azure-sql-database-managed-instance-long-term-backup-retention-powershell"></a>Gerenciar o PowerShell (retenção de backup de longo prazo) da instância gerenciada do banco de dados SQL do Azure

Na instância gerenciada do banco de dados SQL do Azure, você pode configurar uma EPD (política de retenção de backup) de [longo prazo](sql-database-long-term-retention.md#managed-instance-support) como um recurso de visualização pública limitada. Isso permite que você mantenha automaticamente os backups de banco de dados em contêineres separados do armazenamento de BLOBs do Azure por até 10 anos. Em seguida, você pode recuperar um banco de dados usando esses backups com o PowerShell.

   > [!IMPORTANT]
   > A EPD para instâncias gerenciadas está atualmente em visualização limitada e disponível para assinaturas EA e CSP caso a caso. Para solicitar o registro, crie um [tíquete de suporte do Azure](https://azure.microsoft.com/support/create-ticket/). Para tipo de problema, selecione problema técnico, para serviço escolha Instância Gerenciada do Banco de Dados SQL e para o tipo de problema, selecione **backup, restauração e continuidade de negócios/retenção de backup de longo prazo**. Em sua solicitação, informe que você gostaria de estar registrado na visualização pública limitada de EPD para a instância gerenciada.

As seções a seguir mostram como usar o PowerShell para configurar a retenção de backup de longo prazo, exibir backups no armazenamento do SQL do Azure e restaurar a partir de um backup no armazenamento do SQL do Azure.

## <a name="rbac-roles-to-manage-long-term-retention"></a>Funções RBAC para gerenciar retenção de longo prazo

Para **Get-AzSqlInstanceDatabaseLongTermRetentionBackup** e **Restore-AzSqlInstanceDatabase**, você precisará ter uma das seguintes funções:

- Função de proprietário da assinatura ou
- Instância Gerenciada função colaborador ou
- Função personalizada com as seguintes permissões:
  - `Microsoft.Sql/locations/longTermRetentionManagedInstanceBackups/read`
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionManagedInstanceBackups/read`
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/read`

Para **Remove-AzSqlInstanceDatabaseLongTermRetentionBackup**, você precisará ter uma das seguintes funções:

- Função de proprietário da assinatura ou
- Função personalizada com a seguinte permissão:
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/delete`

> [!NOTE]
> A função colaborador de Instância Gerenciada não tem permissão para excluir backups EPD.

As permissões de RBAC podem ser concedidas em escopo de *assinatura* ou *grupo de recursos* . No entanto, para acessar backups EPD que pertencem a uma instância descartada, a permissão deve ser concedida no escopo da *assinatura* dessa instância.

- `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/delete`

## <a name="create-an-ltr-policy"></a>Criar uma política LTR

```powershell
# get the Managed Instance
$subId = "<subscriptionId>"
$instanceName = "<instanceName>"
$resourceGroup = "<resourceGroupName>"
$dbName = "<databaseName>"

Connect-AzAccount
Select-AzSubscription -SubscriptionId $subId

$instance = Get-AzSqlInstance -Name $instanceName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy -InstanceName $instanceName `
   -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetention = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy -InstanceName $instanceName `
    -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

## <a name="view-ltr-policies"></a>Exibir políticas LTR

Este exemplo mostra como listar as políticas EPD dentro de uma instância

```powershell
# gets the current version of LTR policy for the database
$ltrPolicies = Get-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy -InstanceName $instanceName `
    -DatabaseName $dbName -ResourceGroupName $resourceGroup
```

## <a name="clear-an-ltr-policy"></a>Limpar uma política LTR

Este exemplo mostra como limpar uma política LTR de um banco de dados

```powershell
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy -InstanceName $instanceName `
   -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

## <a name="view-ltr-backups"></a>Exibir backups LTR

Este exemplo mostra como listar os backups EPD dentro de uma instância.

```powershell
# get the list of all LTR backups in a specific Azure region
# backups are grouped by the logical database id, within each group they are ordered by the timestamp, the earliest backup first
$ltrBackups = Get-AzSqlInstanceDatabaseLongTermRetentionBackup -Location $instance.Location

# get the list of LTR backups from the Azure region under the given managed instance
$ltrBackups = Get-AzSqlInstanceDatabaseLongTermRetentionBackup -Location $instance.Location -InstanceName $instanceName

# get the LTR backups for a specific database from the Azure region under the given managed instance
$ltrBackups = Get-AzSqlInstanceDatabaseLongTermRetentionBackup -Location $instance.Location -InstanceName $instanceName -DatabaseName $dbName

# list LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzSqlInstanceDatabaseLongTermRetentionBackup -Location $instance.Location -DatabaseState Live

# only list the latest LTR backup for each database
$ltrBackups = Get-AzSqlInstanceDatabaseLongTermRetentionBackup -Location $instance.Location -InstanceName $instanceName -OnlyLatestPerDatabase
```

## <a name="delete-ltr-backups"></a>Excluir backups LTR

Este exemplo mostra como excluir um backup LTR da lista de backups.

```powershell
# remove the earliest backup
$ltrBackup = $ltrBackups[0]
Remove-AzSqlInstanceDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

> [!IMPORTANT]
> A exclusão do backup LTR é irreversível. Para excluir um backup EPD depois que a instância tiver sido excluída, você deverá ter a permissão de escopo da assinatura. Você pode configurar notificações sobre cada exclusão em Azure Monitor filtrando para a operação ' exclui um backup de retenção de longo prazo '. O log de atividades contém informações sobre quem fez a solicitação e quando. Confira [Criar alertas do log de atividades](../azure-monitor/platform/alerts-activity-log.md) para obter instruções detalhadas.

## <a name="restore-from-ltr-backups"></a>Restaurar a partir de backups LTR

Este exemplo mostra como restaurar a partir de um backup LTR. Observe que esta interface não foi alterada, mas o parâmetro de identificação do recurso agora requer a identificação do recurso do backup LTR.

```powershell
# restore a specific LTR backup as an P1 database on the instance $instanceName of the resource group $resourceGroup
Restore-AzSqlInstanceDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId `
   -TargetInstanceName $instanceName -TargetResourceGroupName $resourceGroup -TargetInstanceDatabaseName $dbName
```

> [!IMPORTANT]
> Para restaurar de um backup EPD após a instância ter sido excluída, você deve ter permissões com escopo para a assinatura da instância e essa assinatura deve estar ativa. Você também deve omitir o parâmetro opcional-ResourceGroupName.

> [!NOTE]
> A partir daqui, você pode conectar o banco de dados restaurado usando o SQL Server Management Studio para executar as tarefas necessárias, tais como, extrair um pouco de dados do banco de dados restaurado para copiar para o banco de dados existente ou excluir o banco de dados existente e renomear o banco de dados restaurado com o nome do banco de dados existente. Confira [recuperação pontual](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre backups automáticos gerados pelo serviço, veja [backups automáticos](sql-database-automated-backups.md)
- Para saber mais sobre a retenção de backup de longo prazo, consulte [retenção de backup de longo prazo](sql-database-long-term-retention.md)
