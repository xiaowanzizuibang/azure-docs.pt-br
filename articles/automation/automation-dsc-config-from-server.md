---
title: Criar configurações de servidores existentes-automação do Azure
description: Saiba como criar configurações de servidores existentes para a automação do Azure.
keywords: DSC,powershell,configuração,instalação
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: dff9b8f52207a38cf7eaddefa178aff262ddc546
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80585557"
---
# <a name="create-configurations-from-existing-servers"></a>Criar configurações de servidores existentes

> Aplica-se a: Windows PowerShell 5,1

A criação de configurações de servidores existentes pode ser uma tarefa desafiadora.
Talvez você não queira *todas* as configurações, apenas as que você se importa.
Mesmo assim, você precisa saber em qual ordem as configurações devem ser aplicadas para que a configuração seja aplicada com êxito.

> [!NOTE]
> Este artigo refere-se a uma solução mantida pela comunidade de código-fonte aberto.
> O suporte está disponível apenas na forma de colaboração do GitHub, não da Microsoft.

## <a name="community-project-reversedsc"></a>Projeto da Comunidade: ReverseDSC

Uma solução mantida pela comunidade chamada [ReverseDSC](https://github.com/microsoft/reversedsc) foi criada para funcionar nessa área, iniciando o SharePoint.

A solução se baseia no [recurso SharePointDSC](https://github.com/powershell/sharepointdsc) e a estende para orquestrar a [coleta de informações](https://github.com/Microsoft/sharepointDSC.reverse#how-to-use) de servidores do SharePoint existentes.
A versão mais recente tem vários [modos de extração](https://github.com/Microsoft/SharePointDSC.Reverse/wiki/Extraction-Modes) para determinar o nível de informações a ser incluído.

O resultado do uso da solução é a geração de [dados de configuração](https://github.com/Microsoft/sharepointDSC.reverse#configuration-data) a serem usados com os scripts de configuração do SharePointDSC.

Depois que os arquivos de dados tiverem sido gerados, você poderá usá-los com [scripts de configuração DSC](/powershell/scripting/dsc/overview/overview) para gerar arquivos MOF e [carregar os arquivos MOF para a automação do Azure](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation).
Em seguida, registre os servidores no [local](/azure/automation/automation-dsc-onboarding#onboarding-physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances) ou [no Azure](/azure/automation/automation-dsc-onboarding#onboarding-azure-vms) para efetuar pull das configurações.

Para experimentar o ReverseDSC, visite a [Galeria do PowerShell](https://www.powershellgallery.com/packages/ReverseDSC/) e baixe a solução ou clique em "site do projeto" para exibir a [documentação](https://github.com/Microsoft/sharepointDSC.reverse).

## <a name="next-steps"></a>Próximas etapas

- [Visão Geral da Configuração de Estado Desejado do Windows PowerShell](/powershell/scripting/dsc/overview/overview)
- [Recursos de DSC](/powershell/scripting/dsc/resources/resources)
- [Configurando o Configuration Manager local](/powershell/scripting/dsc/managing-nodes/metaconfig)
