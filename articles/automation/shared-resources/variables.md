---
title: Gerenciar variáveis na automação do Azure
description: Ativos de variáveis são valores que estão disponíveis para todos os runbooks e configurações DSC na Automação do Azure.  Este artigo explica os detalhes das variáveis e como trabalhar com elas na criação de textos e gráficos.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 05/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4778e9b2c0d3b442b214966ab69810d2f42b70b8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81732758"
---
# <a name="manage-variables-in-azure-automation"></a>Gerenciar variáveis na automação do Azure

Ativos de variável são valores que estão disponíveis para todos os runbooks e configurações DSC em sua conta de automação. Você pode gerenciá-los no portal do Azure, no PowerShell, em um runbook ou em uma configuração DSC.

As variáveis de automação são úteis para os seguintes cenários:

- Compartilhando um valor entre vários runbooks ou configurações DSC.

- Compartilhar um valor entre vários trabalhos do mesmo runbook ou configuração DSC.

- Gerenciar um valor usado por runbooks ou configurações DSC no portal ou na linha de comando do PowerShell. Um exemplo é um conjunto de itens de configuração comuns, como uma lista específica de nomes de VM, um grupo de recursos específico, um nome de domínio do AD e muito mais.  

A automação do Azure persiste variáveis e as torna disponíveis mesmo se um runbook ou configuração DSC falhar. Esse comportamento permite que um runbook ou configuração DSC defina um valor que é usado por outro runbook ou pelo mesmo runbook ou configuração DSC na próxima vez em que for executado.

A automação do Azure armazena cada variável criptografada com segurança. Ao criar uma variável, você pode especificar sua criptografia e armazenamento pela automação do Azure como um ativo seguro. Outros ativos seguros incluem credenciais, certificados e conexões. A automação do Azure criptografa esses ativos e os armazena usando uma chave exclusiva que é gerada para cada conta de automação. A chave é armazenada em um Key Vault gerenciado pelo sistema. Antes de armazenar um ativo seguro, a automação do Azure carrega a chave do Key Vault e a usa para criptografar o ativo. 

>[!NOTE]
>Este artigo foi atualizado para usar o novo módulo Az do Azure PowerShell. Você ainda pode usar o módulo AzureRM, que continuará a receber as correções de bugs até pelo menos dezembro de 2020. Para saber mais sobre o novo módulo Az e a compatibilidade com o AzureRM, confira [Apresentação do novo módulo Az do Azure PowerShell](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Para obter instruções de instalação do módulo AZ no seu Hybrid Runbook Worker, consulte [instalar o módulo Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Para sua conta de automação, você pode atualizar seus módulos para a versão mais recente usando [como atualizar os módulos de Azure PowerShell na automação do Azure](../automation-update-azure-modules.md).

## <a name="variable-types"></a>Tipos de variável

Ao criar uma variável com o portal do Azure, você deve especificar um tipo de dados na lista suspensa para que o portal possa exibir o controle apropriado para inserir o valor da variável. A seguir estão os tipos de variáveis disponíveis na automação do Azure:

* Cadeia de caracteres
* Integer
* Datetime
* Booliano
* Null

A variável não é restrita ao tipo de dados designado. Você deve definir a variável usando o Windows PowerShell se desejar especificar um valor de um tipo diferente. Se você indicar `Not defined`, o valor da variável será definido como NULL. Você deve definir o valor com o cmdlet [set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0) ou a `Set-AutomationVariable` atividade.

Você não pode usar o portal do Azure para criar ou alterar o valor de um tipo de variável complexa. No entanto, você pode fornecer um valor de qualquer tipo usando o Windows PowerShell. Tipos complexos são recuperados como um [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject).

Você pode armazenar vários valores para uma única variável criando uma matriz ou hashtable e salvando-a na variável.

>[!NOTE]
>As variáveis de nome da VM podem ter, no máximo, 80 caracteres. As variáveis do grupo de recursos podem ter, no máximo, 90 caracteres. Consulte [regras e restrições de nomenclatura para recursos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-name-rules).

## <a name="powershell-cmdlets-that-create-and-manage-variable-assets"></a>Cmdlets do PowerShell que criam e gerenciam ativos de variável

Para o módulo AZ, os cmdlets na tabela a seguir são usados para criar e gerenciar ativos de variável de automação com o Windows PowerShell. Eles são fornecidos como parte do [módulo AZ. Automation](/powershell/azure/overview), que está disponível para uso em Runbooks de automação e configurações DSC.

| Cmdlet | Descrição |
|:---|:---|
|[Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0) | Recupera o valor de uma variável existente. Você não pode usar este cmdlet para recuperar o valor de uma variável criptografada. A única maneira de fazer isso é usando a `Get-AutomationVariable` atividade em uma configuração de RUNBOOK ou DSC. |
|[New-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationvariable?view=azps-3.5.0) | Cria uma nova variável e define o seu valor.|
|[Remove-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationvariable?view=azps-3.5.0)| Remove uma variável existente.|
|[Set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0)| Define o valor de uma variável existente. |

## <a name="activities-to-access-variables-in-runbooks-and-dsc-configurations"></a>Atividades para acessar variáveis em runbooks e configurações DSC

As atividades na tabela a seguir são usadas para acessar variáveis em runbooks e configurações DSC. Os cmdlets para essas atividades vêm com o módulo `Orchestrator.AssetManagement.Cmdlets`global.

| Atividade | Descrição |
|:---|:---|
|`Get-AutomationVariable`|Recupera o valor de uma variável existente.|
|`Set-AutomationVariable`|Define o valor de uma variável existente.|

> [!NOTE]
> Evite usar variáveis no `Name` parâmetro de `Get-AutomationVariable` um runbook ou configuração DSC. O uso desse parâmetro pode complicar a descoberta de dependências entre runbooks ou configurações DSC e variáveis de automação em tempo de design.

Observe que `Get-AutomationVariable` o não funciona no PowerShell, mas apenas em uma configuração de RUNBOOK ou DSC. Por exemplo, para ver o valor de uma variável criptografada, você pode criar um runbook para obter a variável e, em seguida, gravá-la no fluxo de saída:
 
```powershell
$mytestencryptvar = Get-AutomationVariable -Name TestVariable
Write-output "The encrypted value of the variable is: $mytestencryptvar"
```

## <a name="functions-to-access-variables-in-python-2-runbooks"></a>Funções para acessar variáveis em runbooks do Python 2

As funções na tabela a seguir são usadas para acessar variáveis em um runbook Python 2.

|Funções do Python 2|Descrição|
|:---|:---|
|`automationassets.get_automation_variable`|Recupera o valor de uma variável existente. |
|`automationassets.set_automation_variable`|Define o valor de uma variável existente. |

> [!NOTE]
> Você deve importar o `automationassets` módulo na parte superior do seu runbook do Python para acessar as funções de ativo.

## <a name="working-with-automation-variables"></a>Trabalhando com variáveis de automação

>[!NOTE]
>Se você quiser remover a criptografia de uma variável, deverá excluir a variável e recriá-la como não criptografada.

### <a name="create-a-new-variable-using-the-azure-portal"></a>Criar uma nova variável usando o portal do Azure

1. Na sua conta de automação, clique no bloco **ativos** , na folha **ativos** e selecione **variáveis**.
2. No bloco **Variáveis**, clique em **Adicionar uma variável**.
3. Conclua as opções na folha **nova variável** e, em seguida, clique em **criar** para salvar a nova variável.

> [!NOTE]
> Depois de salvar uma variável criptografada, ela não pode ser exibida no Portal. Ele só pode ser atualizado.

### <a name="create-and-use-a-variable-in-windows-powershell"></a>Criar e usar uma variável no Windows PowerShell

Um script do PowerShell usa `New-AzAutomationVariable` o cmdlet ou seu equivalente do módulo AzureRM, para criar uma nova variável e definir seu valor inicial. Se a variável for criptografada, a chamada deverá usar `Encrypted` o parâmetro.

O script recupera o valor da variável usando [Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0). Se o valor for um tipo simples, o cmdlet recuperará o mesmo tipo. Se for um tipo complexo, um `PSCustomObject` tipo será recuperado.

>[!NOTE]
>Um script do PowerShell não pode recuperar um valor criptografado. A única maneira de fazer isso é usar uma `Get-AutomationVariable` atividade em uma configuração de RUNBOOK ou DSC.

O exemplo a seguir mostra como criar uma variável do tipo cadeia de caracteres e retornar seu valor.

```powershell
New-AzAutomationVariable -ResourceGroupName "ResourceGroup01" 
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
–Encrypted $false –Value 'My String'
$string = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value
```

O exemplo a seguir mostra como criar uma variável com um tipo complexo e, em seguida, recuperar suas propriedades. Nesse caso, um objeto de máquina virtual de [Get-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM?view=azps-3.5.0) é usado.

```powershell
$vm = Get-AzVM -ResourceGroupName "ResourceGroup01" –Name "VM01"
New-AzAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm

$vmValue = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
$vmName = $vmValue.Name
$vmIpAddress = $vmValue.IpAddress
```

### <a name="create-and-use-a-variable-in-a-runbook-or-dsc-configuration"></a>Criar e usar uma variável em um runbook ou configuração DSC

A única maneira de criar uma nova variável de dentro de um runbook ou configuração DSC é usar o `New-AzAutomationVariable` cmdlet ou seu módulo AzureRM equivalente. O script usa esse cmdlet para definir o valor inicial da variável. O script pode então recuperar o valor usando `Get-AzAutomationVariable`. Se o valor for um tipo simples, esse mesmo tipo será recuperado. Se for um tipo complexo, um `PSCustomObject` tipo será recuperado.

>[!NOTE]
>A única maneira de recuperar um valor criptografado é usando a `Get-AutomationVariable` atividade no runbook ou na configuração DSC. 

### <a name="textual-runbook-samples"></a>Exemplos de runbook textual

#### <a name="set-and-retrieve-a-simple-value-from-a-variable"></a>Definir e recuperar um valor simples de uma variável

Os comandos de exemplo a seguir mostram como definir e recuperar uma variável em um runbook textual. Este exemplo pressupõe a criação de variáveis de inteiro `NumberOfIterations` chamadas `NumberOfRunnings` e e uma variável de `SampleMessage`cadeia de caracteres chamada.

```powershell
$NumberOfIterations = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
$NumberOfRunnings = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
$SampleMessage = Get-AutomationVariable -Name 'SampleMessage'

Write-Output "Runbook has been run $NumberOfRunnings times."

for ($i = 1; $i -le $NumberOfIterations; $i++) {
    Write-Output "$i`: $SampleMessage"
}
Set-AzAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)
```

#### <a name="set-and-retrieve-a-variable-in-a-python-2-runbook"></a>Definir e recuperar uma variável em um runbook Python 2

O exemplo a seguir mostra como usar uma variável, definir uma variável e manipular uma exceção para uma variável inexistente em um runbook do Python 2.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a variable
value = automationassets.get_automation_variable("test-variable")
print value

# set a variable (value can be int/bool/string)
automationassets.set_automation_variable("test-variable", True)
automationassets.set_automation_variable("test-variable", 4)
automationassets.set_automation_variable("test-variable", "test-string")

# handle a non-existent variable exception
try:
    value = automationassets.get_automation_variable("nonexisting variable")
except AutomationAssetNotFound:
    print "variable not found"
```

### <a name="graphical-runbook-samples"></a>Exemplos de runbook gráfico

Em um runbook gráfico, você pode adicionar a `Get-AutomationVariable` atividade `Set-AutomationVariable` ou. Basta clicar com o botão direito do mouse na variável no painel Biblioteca do editor gráfico e selecionar a atividade desejada.

![Adicionar variável à tela](../media/variables/runbook-variable-add-canvas.png)

#### <a name="set-values-in-a-variable"></a>Definir valores em uma variável

A imagem a seguir mostra as atividades de exemplo para atualizar uma variável com um valor simples em um runbook gráfico. Neste exemplo, `Get-AzVM` o recupera uma única máquina virtual do Azure e salva o nome do computador em uma variável de cadeia de caracteres de automação existente. Não importa se o [link é um pipeline ou uma sequência](../automation-graphical-authoring-intro.md#links-and-workflow) , pois o código só espera um único objeto na saída.

![Definir variável simples](../media/variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre como conectar atividades na criação gráfica, confira [links na criação gráfica](../automation-graphical-authoring-intro.md#links-and-workflow).
- Para começar a usar runbooks gráficos, consulte [meu primeiro runbook gráfico](../automation-first-runbook-graphical.md).
