---
title: Obter notificações de manutenção para VMs do Azure usando o PowerShell
description: Exibir notificações de manutenção para máquinas virtuais em execução no Azure e iniciar a manutenção de autoatendimento usando o PowerShell.
author: shants123
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/19/2019
ms.author: shants
ms.openlocfilehash: b23c210d7c8a9f1d42e6e1b46e0f7f81bda857b2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77916075"
---
# <a name="handling-planned-maintenance-using-powershell"></a>Manipulando a manutenção planejada usando o PowerShell

**Este artigo se aplica a máquinas virtuais que executam o Linux e o Windows.**

Você pode usar Azure PowerShell para ver quando as VMs estão agendadas para [manutenção](maintenance-notifications.md). As informações de manutenção planejadas estão disponíveis no cmdlet [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) quando você usa o parâmetro `-status`.
  
As informações de manutenção só serão retornadas se houver manutenção planejada. Se não houver nenhuma manutenção agendada que afete a VM, o cmdlet não retornará nenhuma informação de manutenção. 


```powershell
Get-AzVM -ResourceGroupName myResourceGroup -Name myVM -Status
```

As propriedades a seguir são retornadas em MaintenanceRedeployStatus: 

| Valor | Descrição   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Indica se você pode iniciar a manutenção na máquina virtual neste momento |
| PreMaintenanceWindowStartTime         | O início da janela de autoatendimento de manutenção quando você pode iniciar a manutenção na sua VM |
| PreMaintenanceWindowEndTime           | O fim da janela de autoatendimento de manutenção quando você pode iniciar manutenção na sua VM |
| MaintenanceWindowStartTime            | O início da manutenção agendada na qual o Azure inicia a manutenção na sua VM |
| MaintenanceWindowEndTime              | O término da janela de manutenção agendada na qual o Azure inicia a manutenção na sua VM |
| LastOperationResultCode               | O resultado da última tentativa de iniciar a manutenção na VM |



Você também pode obter o status de manutenção para todas as VMs em um grupo de recursos usando [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm), sem especificar a VM.
 
```powershell
Get-AzVM -ResourceGroupName myResourceGroup -Status
```

O exemplo do PowerShell a seguir usa sua ID de assinatura e retorna uma lista de VMs que estão agendadas para manutenção.

```powershell

function MaintenanceIterator
{
    Select-AzSubscription -SubscriptionId $args[0]

    $rgList= Get-AzResourceGroup 

    for ($rgIdx=0; $rgIdx -lt $rgList.Length ; $rgIdx++)
    {
        $rg = $rgList[$rgIdx]        
    $vmList = Get-AzVM -ResourceGroupName $rg.ResourceGroupName 
        for ($vmIdx=0; $vmIdx -lt $vmList.Length ; $vmIdx++)
        {
            $vm = $vmList[$vmIdx]
            $vmDetails = Get-AzVM -ResourceGroupName $rg.ResourceGroupName -Name $vm.Name -Status
              if ($vmDetails.MaintenanceRedeployStatus )
            {
                Write-Output "VM: $($vmDetails.Name)  IsCustomerInitiatedMaintenanceAllowed: $($vmDetails.MaintenanceRedeployStatus.IsCustomerInitiatedMaintenanceAllowed) $($vmDetails.MaintenanceRedeployStatus.LastOperationMessage)"               
            }
          }
    }
}

```

### <a name="start-maintenance-on-your-vm-using-powershell"></a>Iniciar manutenção na sua VM usando o PowerShell

Usando as informações da função na seção anterior, o seguinte iniciará a manutenção em uma VM se **IsCustomerInitiatedMaintenanceAllowed** for definido como true.

```powershell
Restart-AzVM -PerformMaintenance -name $vm.Name -ResourceGroupName $rg.ResourceGroupName 
```

## <a name="classic-deployments"></a>Implantações clássicas

[!INCLUDE [classic-vm-deprecation](../../includes/classic-vm-deprecation.md)]

Caso você ainda tenha VMs herdadas que foram implantadas usando o modelo de implantação clássico, use o PowerShell para consultar VMs e iniciar a manutenção.

Para obter o status de manutenção de uma VM, digite:

```
Get-AzureVM -ServiceName <Service name> -Name <VM name>
```

Para iniciar a manutenção na VM clássica, digite:

```
Restart-AzureVM -InitiateMaintenance -ServiceName <service name> -Name <VM name>
```

## <a name="next-steps"></a>Próximas etapas

Você também pode manipular a manutenção planejada usando o [CLI do Azure](maintenance-notifications-cli.md) ou o [portal](maintenance-notifications-portal.md).
