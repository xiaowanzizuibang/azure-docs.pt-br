---
title: Monitorar alterações de delegação no seu locatário de gerenciamento
description: Saiba como monitorar a atividade de delegação de locatários do cliente para seu locatário de gerenciamento.
ms.date: 03/30/2020
ms.topic: conceptual
ms.openlocfilehash: a4593b34311eca34e4fb68926a3820899ab3f324
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81458804"
---
# <a name="monitor-delegation-changes-in-your-managing-tenant"></a>Monitorar alterações de delegação no seu locatário de gerenciamento

Como um provedor de serviços, talvez você queira estar ciente quando as assinaturas de clientes ou grupos de recursos são delegados para seu locatário por meio [do gerenciamento de recursos delegado do Azure](../concepts/azure-delegated-resource-management.md)ou quando recursos previamente delegados são removidos.

No locatário de gerenciamento, o [log de atividades do Azure](../../azure-monitor/platform/platform-logs-overview.md) rastreia a atividade de delegação no nível do locatário. Essa atividade registrada inclui todas as delegações adicionadas ou removidas de todos os locatários do cliente.

Este tópico explica as permissões necessárias para monitorar a atividade de delegação para seu locatário (em todos os seus clientes) e as práticas recomendadas para fazer isso. Ele também inclui um script de exemplo que mostra um método para consultar e relatar esses dados.

> [!IMPORTANT]
> Todas essas etapas devem ser executadas em seu locatário de gerenciamento, em vez de em qualquer locatário do cliente.

## <a name="enable-access-to-tenant-level-data"></a>Habilitar o acesso a dados no nível do locatário

Para acessar dados do log de atividades no nível do locatário, uma conta deve ser atribuída à função interna do [leitor de monitoramento](../../role-based-access-control/built-in-roles.md#monitoring-reader) no escopo raiz (/). Essa atribuição deve ser executada por um usuário que tenha a função de administrador global com acesso elevado adicional.

### <a name="elevate-access-for-a-global-administrator-account"></a>Elevar o acesso para uma conta de administrador global

Para atribuir uma função no escopo raiz (/), você precisará ter a função de administrador global com acesso elevado. Esse acesso elevado deve ser adicionado somente quando você precisa fazer a atribuição de função e, em seguida, removido quando terminar.

Para obter instruções detalhadas sobre como adicionar e remover a elevação, consulte [elevar o acesso para gerenciar todas as assinaturas e grupos de gerenciamento do Azure](../../role-based-access-control/elevate-access-global-admin.md).

Depois de elevar seu acesso, sua conta terá a função de administrador de acesso do usuário no Azure no escopo raiz. Essa atribuição de função permite exibir todos os recursos e atribuir acesso em qualquer assinatura ou grupo de gerenciamento no diretório, bem como fazer atribuições de função no escopo raiz. 

### <a name="create-a-new-service-principal-account-to-access-tenant-level-data"></a>Criar uma nova conta de entidade de serviço para acessar dados no nível do locatário

Depois de ter elevado o acesso, você pode atribuir as permissões apropriadas a uma conta para que ela possa consultar dados do log de atividades no nível do locatário. Essa conta precisará ter a função interna do [leitor de monitoramento](../../role-based-access-control/built-in-roles.md#monitoring-reader) atribuída no escopo raiz do seu locatário de gerenciamento.

> [!IMPORTANT]
> A concessão de uma atribuição de função no escopo raiz significa que as mesmas permissões serão aplicadas a todos os recursos no locatário.

Como esse é um nível amplo de acesso, recomendamos que você atribua essa função a uma conta de entidade de serviço, em vez de a um usuário individual ou a um grupo. Além disso, recomendamos as seguintes práticas recomendadas:

- [Crie uma nova conta de entidade de serviço](../../active-directory/develop/howto-create-service-principal-portal.md) para ser usada somente para essa função, em vez de atribuir essa função a uma entidade de serviço existente usada para outra automação.
- Certifique-se de que essa entidade de serviço não tenha acesso a nenhum recurso de cliente delegado.
- [Use um certificado para autenticar](../../active-directory/develop/howto-create-service-principal-portal.md#certificates-and-secrets) e [armazená-lo com segurança no Azure Key Vault](../../key-vault/general/best-practices.md).
- Limite os usuários que têm acesso ao Act em nome da entidade de serviço.

Use um dos métodos a seguir para fazer as atribuições de escopo raiz.

#### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

New-AzRoleAssignment -SignInName <yourLoginName> -Scope "/" -RoleDefinitionName "Monitoring Reader"  -ApplicationId $servicePrincipal.ApplicationId 
```

#### <a name="azure-cli"></a>CLI do Azure

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az role assignment create --assignee 00000000-0000-0000-0000-000000000000 --role "Monitoring Reader" --scope "/"
```

### <a name="remove-elevated-access-for-the-global-administrator-account"></a>Remover acesso elevado para a conta de administrador global

Depois de criar sua conta de entidade de serviço e atribuir a função leitor de monitoramento no escopo raiz, certifique-se de [remover o acesso elevado](../../role-based-access-control/elevate-access-global-admin.md#remove-elevated-access) para a conta de administrador global, pois esse nível de acesso não será mais necessário.

## <a name="query-the-activity-log"></a>Consultar o log de atividades

Depois de criar uma nova conta de entidade de serviço com acesso de leitor de monitoramento ao escopo raiz do seu locatário de gerenciamento, você poderá usá-la para consultar e relatar a atividade de delegação em seu locatário. 

[Este script de Azure PowerShell](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/monitor-delegation-changes) pode ser usado para consultar o último dia de atividade e relatórios sobre quaisquer delegações adicionadas ou removidas (ou tentativas que não foram bem-sucedidas). Ele consulta os dados do [log de atividades do locatário](https://docs.microsoft.com/rest/api/monitor/TenantActivityLogs/List) e, em seguida, constrói os seguintes valores para relatar as delegações adicionadas ou removidas:

- **DelegatedResourceId**: a ID da assinatura delegada ou grupo de recursos
- **CustomerTenantId**: a ID de locatário do cliente
- **CustomerSubscriptionId**: a ID de assinatura que foi delegada ou que contém o grupo de recursos que foi delegado
- **CustomerDelegationStatus**: a alteração de status para o recurso delegado (Succeeded ou Failed)
- **EventTimeStamp**: a data e a hora em que a alteração de delegação foi registrada

Ao consultar esses dados, tenha em mente:

- Se vários grupos de recursos forem delegados em uma única implantação, as entradas separadas serão retornadas para cada grupo de recursos.
- As alterações feitas em uma delegação anterior (como atualizar a estrutura de permissão) serão registradas como uma delegação adicionada.
- Conforme observado acima, uma conta deve ter a função interna do leitor de monitoramento no escopo raiz (/) para acessar os dados no nível do locatário.
- Você pode usar esses dados em seus próprios fluxos de trabalho e relatórios. Por exemplo, você pode usar a [API do coletor de dados http (visualização pública)](../../azure-monitor/platform/data-collector-api.md) para registrar dados em Azure monitor de um cliente de API REST e, em seguida, usar [grupos de ação](../../azure-monitor/platform/action-groups.md) para criar notificações ou alertas.

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Azure Lighthouse: Query Tenant Activity Log for registered/unregistered delegations for the past 1 day

$GetDate = (Get-Date).AddDays((-1))

$dateFormatForQuery = $GetDate.ToUniversalTime().ToString("yyyy-MM-ddTHH:mm:ssZ")

# Getting Azure context for the API call
$currentContext = Get-AzContext

# Fetching new token
$azureRmProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
$profileClient = [Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient]::new($azureRmProfile)
$token = $profileClient.AcquireAccessToken($currentContext.Tenant.Id)

$listOperations = @{
    Uri = "https://management.azure.com/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&`$filter=eventTimestamp ge '$($dateFormatForQuery)'"
    Headers = @{
        Authorization = "Bearer $($token.AccessToken)"
        'Content-Type' = 'application/json'
    }
    Method = 'GET'
}
$list = Invoke-RestMethod @listOperations
$showOperations = $list.value

if ($showOperations.operationName.value -eq "Microsoft.Resources/tenants/register/action")
{
    $registerOutputs  = $showOperations | Where-Object -FilterScript {$_.eventName.value -eq "EndRequest" -and $_.resourceType.value -and $_.operationName.value -eq "Microsoft.Resources/tenants/register/action"}
    foreach ($registerOutput in $registerOutputs)
    {
    $registerOutputdata = [pscustomobject]@{
        Event = "An Azure customer has delegated resources to your tenant";
        DelegatedResourceId = $registerOutput.description |%{$_.split('"')[11]};
        CustomerTenantId = $registerOutput.description |%{$_.split('"')[7]};
        CustomerSubscriptionId = $registerOutput.subscriptionId;
        CustomerDelegationStatus = $registerOutput.status.value;
        EventTimeStamp = $registerOutput.eventTimestamp;
        }
        $registerOutputdata | Format-List
    }
}
if ($showOperations.operationName.value -eq "Microsoft.Resources/tenants/unregister/action") 
{
    $unregisterOutputs  = $showOperations | Where-Object -FilterScript {$_.eventName.value -eq "EndRequest" -and $_.resourceType.value -and $_.operationName.value -eq "Microsoft.Resources/tenants/unregister/action"}
    foreach ($unregisterOutput in $unregisterOutputs)
    {
    $unregisterOutputdata = [pscustomobject]@{
        Event = "An Azure customer has removed delegated resources from your tenant";
        DelegatedResourceId = $unregisterOutput.description |%{$_.split('"')[11]};
        CustomerTenantId = $unregisterOutput.description |%{$_.split('"')[7]};
        CustomerSubscriptionId = $unregisterOutput.subscriptionId;
        CustomerDelegationStatus = $unregisterOutput.status.value;
        EventTimeStamp = $unregisterOutput.eventTimestamp;
        }
        $unregisterOutputdata | Format-List
    }
}
else 
{
    Write-Output "No new delegation changes."
}   


```

## <a name="next-steps"></a>Próximas etapas

- Saiba como integrar clientes ao gerenciamento de [recursos delegado do Azure](../concepts/azure-delegated-resource-management.md).
- Saiba mais sobre [Azure monitor](../../azure-monitor/index.yml) e o [log de atividades do Azure](../../azure-monitor/platform/platform-logs-overview.md).
