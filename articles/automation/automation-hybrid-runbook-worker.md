---
title: Hybrid Runbook Worker da Automação do Azure
description: Este artigo fornece informações sobre como instalar e usar o Hybrid Runbook Worker, que é um recurso do Azure Automation que você pode usar para executar runbooks em máquinas no datacenter local ou no provedor de nuvem.
services: automation
ms.subservice: process-automation
ms.date: 04/05/2019
ms.topic: conceptual
ms.openlocfilehash: 861c7ffa647c8d2f37b32c359253ca991eeb314f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81457698"
---
# <a name="automate-resources-in-your-datacenter-or-cloud-by-using-hybrid-runbook-worker"></a>Automatize recursos em seu datacenter ou nuvem usando o Hybrid Runbook Worker

Os runbooks na Automação do Azure talvez não tenham acesso aos recursos em outras nuvens ou no seu ambiente local por serem executados na plataforma de nuvem do Azure. É possível usar o recurso Hybrid Runbook Worker da Automação do Azure para executar os runbooks diretamente no computador que hospeda a função e os recursos do ambiente para gerenciar esses recursos locais. Runbooks são armazenados e gerenciados na Automação do Azure e, em seguida, entregues a um ou mais computadores atribuídos.

A imagem a seguir ilustra essa funcionalidade:

![Visão geral do Hybrid Runbook Worker](media/automation-hybrid-runbook-worker/automation.png)

Cada Runbook Worker Híbrido é membro de um grupo de Runbook Worker Híbrido que você especifica ao instalar o agente. Um grupo pode conter um único agente, mas você pode instalar vários agentes em um grupo para ter alta disponibilidade. Cada computador pode hospedar um Hybrid Worker relatórios para uma conta de automação.

Quando você inicia um runbook em um Runbook Worker Híbrido, deve especificar o grupo no qual ele será executado. Cada operador no grupo de sonda de automação do Azure para ver se todos os trabalhos estão disponíveis. Se um trabalho estiver disponível, o primeiro funcionário a obter o trabalho o fará. O tempo de processamento da fila de trabalhos depende do perfil de hardware do Hybrid worker e a carga. Você não pode especificar um trabalhador específico. Hybrid Runbook Workers não compartilham muitos dos limites que têm áreas restritas do Azure. Eles não têm os mesmos limites de espaço em disco, memória ou soquetes de rede. Hybrid Runbook Workers são limitadas apenas pelos recursos em Hybrid Runbook Worker em si. Além disso, Hybrid runbook Workers não compartilham o limite de tempo de [compartilhamento razoável](automation-runbook-execution.md#fair-share) de 180 minutos que as áreas restritas do Azure têm. Para saber mais sobre os limites de serviço para as áreas restritas do Azure e Hybrid runbook Workers, consulte os [limites](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)de trabalho.

## <a name="install-a-hybrid-runbook-worker"></a>Instalar um trabalhador de runbook híbrido

O processo para instalar uma Hybrid Runbook Worker depende do sistema operacional. A tabela a seguir define os tipos de implantação.

|SO  |Tipos de implantação  |
|---------|---------|
|Windows     | [PowerShell](automation-windows-hrw-install.md#automated-deployment)<br>[Manual](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker)        |

O método de instalação recomendado é usar um runbook de automação para automatizar completamente o processo de configuração de um computador com Windows. O segundo método é seguir um procedimento passo a passo para instalar e configurar manualmente a função. Para máquinas Linux, você executa um script Python para instalar o agente na máquina.

> [!NOTE]
> Para gerenciar a configuração dos servidores que dão suporte à função de Hybrid Runbook Worker com a DSC (configuração de estado desejado), você precisa adicionar os servidores como nós DSC. Para saber mais sobre a integração deles para gerenciamento com DSC, confira [Máquinas de integração para o gerenciamento pelo DSC de Automação do Azure](automation-dsc-onboarding.md).
>
>Se você habilitar a [ solução de Gerenciamento de Atualizações ](automation-update-management.md), qualquer computador conectado ao seu espaço de trabalho do Log Analytics do Azure será automaticamente configurado como um Operador de Runbook Híbrido para oferecer suporte a runbooks incluídos nessa solução. No entanto, o computador não está registrado em nenhum grupo de Hybrid Worker já definido em sua conta de automação. O computador pode ser adicionado a um grupo de executável de manual híbrido em sua conta de automação para oferecer suporte a registros de execução de automação, desde que você esteja usando a mesma conta para a solução e a associação de grupo de trabalhador de executável híbrido. Esta funcionalidade foi adicionada à versão 7.2.12024.0 do Hybrid Runbook Worker.

Examine o [informações para planejar sua rede](#network-planning) antes de você começar a implantar um Hybrid Runbook Worker. Depois de implantar com êxito o trabalhador, revise [Executar runbooks em um Hybrid Runbook Worker](automation-hrw-run-runbooks.md) para saber como configurar seus runbooks para automatizar processos em seu datacenter local ou em outro ambiente de nuvem.

O computador pode ser adicionado a um grupo de executável de manual híbrido em sua conta de automação para oferecer suporte a registros de execução de automação, desde que você esteja usando a mesma conta para a solução e a associação de grupo de trabalhador de executável híbrido. Essa funcionalidade foi adicionada à versão 7.2.12024.0 do Hybrid Runbook Worker.

## <a name="a-nameremove-a-hybrid-runbook-workerremove-a-hybrid-runbook-worker-from-an-on-premises-computer"></a><a name="remove-a-hybrid-runbook-worker">Remover um Hybrid Runbook Worker de um computador local

Você pode remover um Hybrid Runbook Worker de um computador local, conforme descrito nesta seção para Windows e Linux.

### <a name="remove-the-worker-on-windows"></a>Remover o trabalhador no Windows

1. No portal do Azure, vá para sua conta de automação.
2. Em **Configurações da conta**, selecione **Chaves** e anote os valores para **URL** e **Chave de acesso primária**.

3. Abra uma sessão do PowerShell no modo de administrador e execute o comando a seguir com a URL e os valores de chave de acesso primário. Use o `Verbose` parâmetro para um log detalhado do processo de remoção. Para remover computadores antigos do grupo do Hybrid Worker, use o parâmetro opcional `machineName`.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey> -machineName <ComputerName>
```

### <a name="remove-the-worker-on-linux"></a>Remover o trabalhador no Linux

Você pode usar o comando `ls /var/opt/microsoft/omsagent` no Hybrid runbook Worker para obter a ID do espaço de trabalho. É criada uma pasta que é nomeada com a ID do espaço de trabalho.

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> Esse código não remove o agente de Log Analytics para Linux do computador. Ele apenas remove a funcionalidade e a configuração da função Hybrid Runbook Worker.

## <a name="remove-a-hybrid-worker-group"></a>Remover um grupo de Hybrid Worker

Para remover um grupo de Hybrid Runbook Worker, primeiro você precisa remover o Hybrid Runbook Worker de cada computador que seja membro do grupo. Em seguida, use as seguintes etapas para remover o grupo:

1. Abra a conta de Automação no Portal do Azure.
2. Selecione **grupos de trabalho híbrido** em **automação de processo**. Selecione o grupo que você deseja excluir. A página de propriedades desse grupo é exibida.

   ![Página Propriedades](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

3. Na página de propriedades do grupo selecionado, selecione **Excluir**. Uma mensagem pede para você confirmar esta ação. Selecione **Sim** se tiver certeza de que deseja continuar.

   ![Mensagem de confirmação](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Esse processo pode levar vários segundos para terminar. Você pode acompanhar o progresso em **Notificações** no menu.

## <a name="configure-your-network"></a><a name="network-planning"></a>Configurar sua rede

### <a name="hybrid-worker-role"></a>Função de trabalhador híbrido

Para que o Hybrid Runbook Worker se conecte e se registre na automação do Azure, ele deve ter acesso ao número da porta e às URLs descritas nesta seção. Esse acesso está no topo das [portas e URLs necessárias para o log Analytics Agent](../azure-monitor/platform/agent-windows.md) se conectar a logs de Azure monitor.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Se você usar um servidor proxy para comunicação entre o agente e o serviço de automação do Azure, verifique se os recursos apropriados estão acessíveis. O tempo limite para solicitações da Hybrid Runbook Worker e dos serviços de automação é de 30 segundos. Após 3 tentativas, a solicitação falhará. Se você usar um firewall para restringir o acesso à Internet, precisará configurar o firewall para permitir o acesso. Se você usar o gateway do Log Analytics como um proxy, verifique se ele está configurado para hybrid workers. Para obter instruções sobre como fazer isso, confira [Configurar o Gateway do Log Analytics para Hybrid Workers de Automação](https://docs.microsoft.com/azure/log-analytics/log-analytics-oms-gateway).

A porta e URLs a seguir são necessárias para a função do Hybrid Runbook Worker se comunicar com a Automação do Azure:

* Porta: Somente a TCP 443 é necessária para acesso de Internet de saída.
* URL global: *.azure-automation.net
* URL global do EUA Gov Virgínia: *.azure automation.us
* Serviço de agente: https://\<workspaceId\>.agentsvc.azure-automation.net

É recomendável usar os endereços listados ao definir exceções. Para endereços IP, você pode baixar os [intervalos de IP do Microsoft Azure datacenter](https://www.microsoft.com/en-us/download/details.aspx?id=56519). Esse arquivo é atualizado semanalmente e tem os intervalos atualmente implantados e as alterações futuras nos intervalos de IP.

Se você tiver uma conta de Automação do Azure definida para uma região específica, você pode restringir a comunicação para esse centro de dados regional. A tabela a seguir fornece o registro DNS para cada região:

| **Região** | **Registro DNS** |
| --- | --- |
| Centro-Oeste dos EUA | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| Centro-Sul dos Estados Unidos |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| Leste dos EUA 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Oeste dos EUA 2 |wus2-jobruntimedata-prod-su1.azure-automation.net</br>wus2-agentservice-prod-1.azure-automation.net |
| Canadá Central |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Europa Ocidental |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Norte da Europa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Sudeste da Ásia |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Índia Central |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Leste do Japão |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Austrália Central |ac-jobruntimedata-prod-su1.azure-automation.net</br>ac-agentservice-prod-1.azure-automation.net |
| Leste da Austrália |ae-jobruntimedata-prod-su1.azure-automation.net</br>ae-agentservice-prod-1.azure-automation.net |
| Sudeste da Austrália |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Sul do Reino Unido | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| Gov. dos EUA – Virgínia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

Para obter uma lista de endereços IP da região em vez de nomes da região, faça o download do arquivo XML do [Endereço IP do Centro de Dados do Azure](https://www.microsoft.com/download/details.aspx?id=41653) do Centro de Download da Microsoft.

> [!NOTE]
> O arquivo XML do endereço IP do centro de dados do Azure lista os intervalos de endereços IP que são usados nos centros de dados do Microsoft Azure. O arquivo inclui intervalos de computação, SQL e armazenamento.
>
>Um arquivo atualizado é postado semanalmente. O arquivo reflete os intervalos atualmente implantados e quaisquer alterações futuras para os intervalos de IP. Novos intervalos que aparecem no arquivo não são usados nos centros de dados por pelo menos uma semana.
>
> É uma boa ideia fazer o download do novo arquivo XML toda semana. Em seguida, atualize seu site para identificar corretamente os serviços em execução no Azure. Os usuários do Azure ExpressRoute devem observar que esse arquivo é usado para atualizar o anúncio BGP (Border Gateway Protocol) do espaço do Azure na primeira semana de cada mês.

### <a name="update-management"></a>Gerenciamento de atualizações

Na parte superior dos endereços padrão e portas que exige o Hybrid Runbook Worker, os endereços a seguir são necessários especificamente para Gerenciamento de Atualizações. A comunicação para esses endereços é feita pela porta 443.

|Público do Azure  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|

## <a name="next-steps"></a>Próximas etapas

* Para saber como configurar os runbooks para automatizar processos no datacenter local ou em outro ambiente de nuvem, consulte [Executar runbooks em um Hybrid Runbook Worker](automation-hrw-run-runbooks.md).
* Para saber como solucionar problemas de seus Hybrid runbook Workers, consulte [Solucionando problemas de Hybrid runbook Workers](troubleshoot/hybrid-runbook-worker.md#general).