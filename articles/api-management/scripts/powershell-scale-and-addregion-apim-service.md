---
title: Exemplo de script do Azure PowerShell – Escalar a instância de serviço | Microsoft Docs
description: Exemplo de script do Azure PowerShell – Escalar a instância de serviço
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.topic: sample
ms.date: 11/16/2017
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: 9bd584c4759d8a4abeae32983fcb60ac8102bb36
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "70073272"
---
# <a name="scale-the-service-instance"></a>Escalar a instância de serviço

Esse script de exemplo escala e adiciona a região à instância de serviço de Gerenciamento de API.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se você escolher instalar e usar o PowerShell localmente, este tutorial exigirá o módulo do Azure PowerShell versão 1.0 ou posterior. Execute `Get-Module -ListAvailable Az` para encontrar a versão. Se você precisa atualizar, consulte [Instalar o módulo do Azure PowerShell](/powershell/azure/install-Az-ps). Se você estiver executando o PowerShell localmente, também precisará executar o `Connect-AzAccount` para criar uma conexão com o Azure.

## <a name="sample-script"></a>Exemplo de script

[!code-powershell[main](../../../powershell_scripts/api-management/scale-and-addregion-apim-service/scale_and_addregion_apim_service.ps1 "Scale the APIM service instance")]

## <a name="clean-up-resources"></a>Limpar os recursos

Quando não forem mais necessários, você poderá usar o comando [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) para remover o grupo de recursos e todos os recursos relacionados.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre o módulo do Azure PowerShell, confira [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

É possível encontrar mais exemplos do Azure PowerShell para o Gerenciamento de API do Azure nos [exemplos do PowerShell](../powershell-samples.md).
