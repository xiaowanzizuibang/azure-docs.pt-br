---
title: Selecionar e implantar agentes de segurança
description: Saiba mais sobre como selecionar e implantar a central de segurança do Azure para agentes de segurança de IoT em dispositivos IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: c3b514c79be87cd136375b4853226426965f4185
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81311209"
---
# <a name="select-and-deploy-a-security-agent-on-your-iot-device"></a>Selecionar e implantar um agente de segurança em seu dispositivo IoT

A central de segurança do Azure para IoT fornece arquiteturas de referência para agentes de segurança que monitoram e coletam dados de dispositivos IoT.
Para saber mais, consulte [arquitetura de referência do agente de segurança](security-agent-architecture.md).

Os agentes são desenvolvidos como projetos de código-fonte aberto e estão disponíveis em dois tipos: <br> [C](https://aka.ms/iot-security-github-c)e [C#](https://aka.ms/iot-security-github-cs).

Neste artigo, você aprenderá como:

> [!div class="checklist"]
> * Comparar tipos de agente de segurança
> * Descobrir plataformas de agente com suporte
> * Escolha o tipo de agente correto para sua solução

## <a name="understand-security-agent-options"></a>Entender as opções do agente de segurança

Cada tipo de agente de segurança da central de segurança do Azure para IoT oferece o mesmo conjunto de recursos e oferece suporte a opções de configuração semelhantes.

O agente de segurança baseado em C tem uma superfície de memória menor e é a opção ideal para dispositivos com menos recursos disponíveis.

|     | Agente de segurança baseado em C | Agente de segurança baseado em C# |
| --- | ----------- | --------- |
| Código-fonte aberto | Disponível sob [licença do MIT](https://en.wikipedia.org/wiki/MIT_License) no [GitHub](https://aka.ms/iot-security-github-cs) | Disponível sob [licença do MIT](https://en.wikipedia.org/wiki/MIT_License) no [GitHub](https://aka.ms/iot-security-github-c) |
| Linguagem de desenvolvimento    | C | C# |
| Plataformas Windows com suporte? | Não | Sim |
| Pré-requisitos do Windows | --- | [WMI](https://docs.microsoft.com/windows/desktop/wmisdk/) |
| Plataformas Linux com suporte? | Sim, x64 e x86 | Sim, somente x64 |
| Pré-requisitos do Linux | libunwind8, libcurl3, UUID-Runtime, audited, audispd-plugins | libunwind8, libcurl3, UUID-Runtime, audited, audispd-plugins, sudo, netstat, iptables |
| Espaço em disco | 10,5 MB | 90 MB |
| Superfície de memória (em média) | 5,5 MB | 33 MB |
| [Autenticação](concept-security-agent-authentication-methods.md) para o Hub IOT | Sim | Sim |
| [Coleta](how-to-agent-configuration.md#supported-security-events) de dados de segurança | Sim | Sim |
| Agregação de eventos | Sim | Sim |
| Configuração remota por meio do [módulo de segurança](concept-security-module.md) ... | Sim | Sim |
|

## <a name="security-agent-installation-guidelines"></a>Diretrizes de instalação do agente de segurança

Para **Windows**: o script install SecurityAgent. ps1 deve ser executado em uma janela administrador do PowerShell.

Para **Linux**: o InstallSecurityAgent.sh deve ser executado como superusuário. É recomendável prefixar o comando de instalação com "sudo".

## <a name="choose-an-agent-flavor"></a>Escolher um tipo de agente

Responda às seguintes perguntas sobre seus dispositivos IoT para selecionar o agente correto:

- Você está usando o _Windows Server_ ou o _Windows IOT Core_?

    [Implante um agente de segurança baseado em C# para Windows](how-to-deploy-windows-cs.md).

- Você está usando uma distribuição do Linux com arquitetura x86?

    [Implante um agente de segurança baseado em C para Linux](how-to-deploy-linux-c.md).

- Você está usando uma distribuição do Linux com arquitetura x64?

    Ambos os tipos de agente podem ser usados. <br>
    [Implante um agente de segurança baseado em C para Linux](how-to-deploy-linux-c.md) e/ou [implante um agente de segurança baseado em C# para Linux](how-to-deploy-linux-cs.md).

Ambos os tipos de agente oferecem o mesmo conjunto de recursos e oferecem suporte a opções de configuração semelhantes.
Consulte [comparação do agente de segurança](how-to-deploy-agent.md#understand-security-agent-options) para saber mais.

## <a name="supported-platforms"></a>Plataformas com Suporte

A lista a seguir inclui todas as plataformas com suporte no momento.

|Central de segurança do Azure para agente de IoT |Sistema operacional |Arquitetura |
|--------------|------------|--------------|
|C|Ubuntu 16.04 |    x64|
|C|Ubuntu 18.04 |    x64, ARMv7|
|C|Debian 9 |    x64, x86|
|C#|Ubuntu 16.04     |x64|
|C#|Ubuntu 18.04    |x64, ARMv7|
|C#|Debian 9    |x64|
|C#|Windows Server 2016|    X64|
|C#|Windows 10 IoT Core, Build 17763    |x64|
|

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre as opções de configuração, prossiga para o guia de instruções para a configuração do agente.
> [!div class="nextstepaction"]
> [Guia de instruções de configuração do agente](./how-to-agent-configuration.md)
