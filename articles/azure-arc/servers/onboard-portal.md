---
title: Conectar computadores híbridos ao Azure por meio do portal do Azure
description: Neste artigo, você aprenderá a instalar o agente e a conectar computadores ao Azure usando o Azure Arc para servidores (versão prévia) no portal do Azure.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 03/24/2020
ms.topic: conceptual
ms.openlocfilehash: 40885e1de4ff4c16d2a50399c654d8596396ab53
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80366375"
---
# <a name="connect-hybrid-machines-to-azure-from-the-azure-portal"></a>Conectar computadores híbridos ao Azure por meio do portal do Azure

Você pode habilitar o Azure Arc para servidores (versão prévia) para um ou um pequeno número de computadores Windows ou Linux em seu ambiente executando um conjunto de etapas manualmente. Ou você pode usar um método automatizado executando um script de modelo que fornecemos. Esse script automatiza o download e a instalação de ambos os agentes.

Esse método exige que você tenha permissões de administrador no computador para instalar e configurar o agente. No Linux, usando a conta raiz, e no Windows, você é membro do grupo local de administradores.

Antes de começar, examine os [pré-requisitos](overview.md#prerequisites) e verifique se a sua assinatura e os recursos atendem aos requisitos.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="generate-the-installation-script-from-the-azure-portal"></a>Gerar o script de instalação no portal do Azure

O script usado para automatizar o download e a instalação e para estabelecer a conexão com o Azure Arc está disponível no portal do Azure. Para concluir o processo, faça o seguinte:

1. No navegador, acesse o [portal do Azure](https://aka.ms/hybridmachineportal).

1. Na página **Computadores – Azure Arc**, selecione **Adicionar** no canto superior esquerdo ou a opção **Criar computador – Azure Arc** na parte inferior do painel central. 

1. Na página **Selecionar um método**, selecione o bloco **Adicionar computadores usando o script interativo** e, em seguida, selecione **Gerar script**.

1. Na página **Gerar script**, selecione a assinatura e o grupo de recursos nos quais você deseja que o computador seja gerenciado no Azure. Selecione uma localização do Azure em que os metadados do computador serão armazenados.

    >[!NOTE]
    >O Azure Arc para servidores (versão prévia) só dá suporte às seguintes regiões:
    >- WestUS2
    >- WestEurope
    >- WestAsia
    >
    >Examine considerações adicionais ao selecionar uma região [aqui](overview.md#supported-regions) no artigo de visão geral.

1. Na página **Gerar script**, na lista suspensa **Sistema operacional**, selecione o sistema operacional no qual o script será executado.

1. Se a máquina estiver se comunicando por meio de um servidor proxy para se conectar à Internet, selecione **Avançar: servidor proxy**. 
1. Na guia **Servidor proxy**, especifique o endereço IP do servidor proxy ou o nome e o número da porta que o computador usará para se comunicar com o servidor proxy. Digite o valor no formato `http://<proxyURL>:<proxyport>`. 
1. Selecione **Examinar + gerar**.

1. Na guia **Examinar + gerar**, examine as informações de resumo e, em seguida, selecione **Baixar**. Se você ainda precisar fazer alterações, selecione **Anterior**.

## <a name="install-and-validate-the-agent-on-windows"></a>Instalar e validar o agente no Windows

### <a name="install-manually"></a>Instalar manualmente

Instale o agente do Connected Machine manualmente executando o pacote *AzureConnectedMachineAgent.msi* do Windows Installer. 

> [!NOTE]
> * Para instalar ou desinstalar o agente, você precisa ter permissões de *Administrador*.
> * Primeiro, é necessário baixar e copiar o pacote do instalador para uma pasta no servidor de destino ou de uma pasta de rede compartilhada. Se você executar o pacote do instalador sem nenhuma opção, ele iniciará um assistente de instalação que você poderá seguir para instalar o agente de maneira interativa.

Se o computador precisar se comunicar por meio de um servidor proxy com o serviço, depois de instalar o agente, você precisará executar um comando que será descrito mais adiante neste artigo. Isso define a variável de ambiente `https_proxy` do sistema do servidor proxy.

Se você não estiver familiarizado com as opções de linha de comando para pacotes Windows Installer, examine as opções de [linha de comando MSIExec padrão](https://docs.microsoft.com/windows/win32/msi/standard-installer-command-line-options) e [Opções de linha de comando msiexec](https://docs.microsoft.com/windows/win32/msi/command-line-options).

Por exemplo, execute o programa de instalação com `/?` o parâmetro para examinar a opção ajuda e referência rápida. 

```dos
msiexec.exe /i AzureConnectedMachineAgent.msi /?
```

Para instalar o agente silenciosamente e criar um arquivo de log da instalação `C:\Support\Logs` na pasta, execute o comando a seguir.

```dos
msiexec.exe /i AzureConnectedMachineAgent.msi /qn /l*v "C:\Support\Logs\Azcmagentsetup.log"
```

Os arquivos para o agente do computador conectado são instalados por padrão em *C:\Program Files\AzureConnectedMachineAgent*. Se o agente não for iniciado após a conclusão da instalação, verifique os logs para obter informações de erro detalhadas. O diretório de log é *%Programfiles%\AzureConnectedMachineAgentAgent\logs*.

### <a name="install-with-the-scripted-method"></a>Instalação com o método com script

1. Faça logon no servidor.

1. Abra um prompt de comando com privilégios elevados do PowerShell.

1. Vá para a pasta ou o compartilhamento para o qual você copiou o script e execute-o no servidor executando o script `./OnboardingScript.ps1`.

### <a name="configure-the-agent-proxy-setting"></a>Definir a configuração do proxy do agente

Para definir a variável de ambiente do servidor proxy, execute o seguinte comando:

```powershell
# If a proxy server is needed, execute these commands with the proxy URL and port.
[Environment]::SetEnvironmentVariable("https_proxy", "http://{proxy-url}:{proxy-port}", "Machine")
$env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
# For the changes to take effect, the agent service needs to be restarted after the proxy environment variable is set.
Restart-Service -Name himds
```

>[!NOTE]
>O agente não dá suporte à definição da autenticação de proxy nesta versão prévia.
>

### <a name="configure-agent-communication"></a>Configurar a comunicação do agente

Depois de instalar o agente, você precisará configurá-lo para se comunicar com o serviço Azure Arc executando o seguinte comando:

`%ProgramFiles%\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "<resourceGroupName>" --tenant-id "<tenantID>" --location "<regionName>" --subscription-id "<subscriptionID>"`

## <a name="install-and-validate-the-agent-on-linux"></a>Instalar e validar o agente no Linux

O agente do Connected Machine para Linux é fornecido no formato de pacote preferencial para a distribuição (.RPM ou .DEB) hospedada no [repositório de pacotes](https://packages.microsoft.com/) da Microsoft. O [pacote de script do shell `Install_linux_azcmagent.sh`](https://aka.ms/azcmagent) executa as seguintes ações:

- Configura o computador host para baixar o pacote do agente em packages.microsoft.com.
- Instala o pacote do provedor de recursos híbrido.

Opcionalmente, você pode configurar o agente com as suas informações de proxy, incluindo o parâmetro `--proxy "{proxy-url}:{proxy-port}"`.

O script também contém a lógica para identificar as distribuições com e sem suporte e verifica as permissões necessárias para executar a instalação. 

O seguinte exemplo baixa o agente e o instala:

```bash
# Download the installation package.
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent. 
bash ~/Install_linux_azcmagent.sh
```

Para baixar e instalar o agente, incluindo o parâmetro `--proxy` para configurar o agente para se comunicar por meio do servidor proxy, execute os seguintes comandos:

```bash
# Download the installation package.
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent. 
bash ~/Install_linux_azcmagent.sh --proxy "{proxy-url}:{proxy-port}"
```

### <a name="configure-the-agent-communication"></a>Configurar a comunicação do agente

Depois de instalar o agente, configure-o para se comunicar com o serviço Azure Arc executando o seguinte comando:

`/opt/azcmagent/bin/azcmagent.exe" connect --resource-group "<resourceGroupName>" --tenant-id "<tenantID>" --location "<regionName>" --subscription-id "<subscriptionID>"`

## <a name="verify-the-connection-with-azure-arc"></a>Verificar a conexão com o Azure Arc

Depois de instalar o agente e configurá-lo para se conectar ao Azure Arc para servidores (versão prévia), acesse o portal do Azure para verificar se o servidor foi conectado com êxito. Veja seus computadores no [portal do Azure](https://aka.ms/hybridmachineportal).

![Uma conexão de servidor bem-sucedida](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Próximas etapas

- Saiba como gerenciar seu computador usando [Azure Policy](../../governance/policy/overview.md), para tarefas como [configuração de convidado](../../governance/policy/concepts/guest-configuration.md)de VM, verificar se o computador está relatando para o espaço de trabalho esperado log Analytics, habilitar o monitoramento com o [Azure monitor com VMs](../../azure-monitor/insights/vminsights-enable-at-scale-policy.md)e muito mais.

- Saiba mais sobre o [agente de log Analytics](../../azure-monitor/platform/log-analytics-agent.md). O agente do Log Analytics para Windows e Linux é necessário quando você deseja monitorar proativamente o sistema operacional e as cargas de trabalho em execução no computador, o gerencia usando os runbooks de automação ou soluções como o Gerenciamento de Atualizações ou usa outros serviços do Azure como a [Central de Segurança do Azure](../../security-center/security-center-intro.md).
