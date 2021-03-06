---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 02/24/2020
ms.topic: include
ms.openlocfilehash: c77849b2285283a34e6adf84dc3845a4076407af
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77597926"
---
## <a name="attack-scenario"></a>Cenário de ataque

Ataques de força bruta geralmente se destinam às portas de gerenciamento como um meio para obter acesso a uma VM. Se for bem-sucedido, um invasor poderá assumir o controle sobre a VM e estabelecer uma base em seu ambiente.

Uma maneira de reduzir a exposição a um ataque de força bruta é limitar a quantidade de tempo que uma porta fica aberta. As portas de gerenciamento não precisam ser abertas o tempo todo. Eles só precisam ser abertos enquanto você estiver conectado à VM, por exemplo, para executar tarefas de gerenciamento ou manutenção. Quando o just-in-time está habilitado, a central de segurança usa o NSG ( [grupo de segurança de rede](../articles/virtual-network/security-overview.md#security-rules) ) e as regras de firewall do Azure, que restringem o acesso às portas de gerenciamento para que não possam ser direcionadas por invasores

![Cenário JIT](../articles/security-center/media/security-center-just-in-time/just-in-time-scenario.png)

## <a name="how-does-jit-access-work"></a>Como funciona o acesso JIT?

Quando o JIT está habilitado, a Central de Segurança bloqueia o tráfego de entrada às suas VMs do Azure, criando uma regra de NSG. Você seleciona as portas na VM para as quais o tráfego de entrada será bloqueado. Essas portas são controladas pela solução Just-In-Time.

Quando um usuário solicita acesso a uma VM, a central de segurança verifica se o usuário tem permissões de [RBAC (controle de acesso baseado em função)](../articles/role-based-access-control/role-assignments-portal.md) para essa VM. Se a solicitação for aprovada, a central de segurança configurará automaticamente os NSGs (grupos de segurança de rede) e o Firewall do Azure para permitir o tráfego de entrada para as portas selecionadas e os endereços ou intervalos de IP de origem solicitados, para o período de tempo especificado. Depois que o tempo expirar, a Central de Segurança restaura os NSGs aos seus estados anteriores. No entanto, as conexões já estabelecidas não estão sendo interrompidas.

 > [!NOTE]
 > Se uma solicitação de acesso JIT for aprovada para uma VM por trás de um firewall do Azure, a central de segurança alterará automaticamente as regras de política de firewall e NSG. Para o período de tempo especificado, as regras permitem o tráfego de entrada para as portas selecionadas e os endereços ou intervalos IP de origem solicitados. Após o tempo, a central de segurança restaura as regras de firewall e NSG para seus Estados anteriores.


## <a name="permissions-needed-to-configure-and-use-jit"></a>Permissões necessárias para configurar e usar o JIT

| Para permitir que um usuário: | Permissões a serem definidas|
| --- | --- |
| Configurar ou editar uma política JIT para uma VM | *Atribua essas ações à função:*  <ul><li>No escopo de uma assinatura ou grupo de recursos associado à VM:<br/> `Microsoft.Security/locations/jitNetworkAccessPolicies/write` </li><li> No escopo de uma assinatura ou grupo de recursos da VM: <br/>`Microsoft.Compute/virtualMachines/write`</li></ul> | 
|Solicitar acesso JIT a uma VM | *Atribua essas ações ao usuário:*  <ul><li>No escopo de uma assinatura ou grupo de recursos associado à VM:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action` </li><li>No escopo de uma assinatura ou grupo de recursos associado à VM:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/*/read` </li><li>  No escopo de uma assinatura ou grupo de recursos ou VM:<br/> `Microsoft.Compute/virtualMachines/read` </li><li>  No escopo de uma assinatura ou grupo de recursos ou VM:<br/> `Microsoft.Network/networkInterfaces/*/read` </li></ul>|