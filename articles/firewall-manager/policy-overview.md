---
title: Visão geral da política de versão prévia do Gerenciador de Firewall do Azure
description: Saiba mais sobre as políticas do Gerenciador de Firewall do Azure
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: overview
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: 1308f4ba3335f2fd2633f6e39a679cd6477a4b5c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77444946"
---
# <a name="azure-firewall-manager-preview-policy-overview"></a>Visão geral da política de versão prévia do Gerenciador de Firewall do Azure

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

A política de firewall é um recurso do Azure que contém coleções de regra de aplicativo, de rede e de NAT, bem como configurações de inteligência contra ameaças. É um recurso global que pode ser usado em várias instâncias do Firewall do Azure em hubs virtuais seguros e redes virtuais de hub. As políticas funcionam em regiões e assinaturas diferentes.

![Política do Gerenciador de Firewall do Azure](media/policy-overview/policy-overview.png)

## <a name="policy-creation-and-association"></a>Criação e associação de política

Uma política pode ser criada e gerenciada de várias maneiras, incluindo o portal do Azure, a API REST, os modelos, o Azure PowerShell e a CLI.

Você também pode migrar regras existentes do Firewall do Azure usando o portal ou o Azure PowerShell para criar políticas. Para obter mais informações, confira [como migrar configurações do Firewall do Azure para a Política de Firewall do Azure (versão prévia)](migrate-to-policy.md). 

As políticas podem ser associadas a um ou mais hubs virtuais ou VNets. O firewall pode estar em qualquer assinatura associada à sua conta e em qualquer região.

## <a name="hierarchical-policies"></a>Políticas hierárquicas

Políticas podem ser criadas do zero ou herdadas de políticas existentes. A herança permite que o DevOps crie políticas de firewall locais sobre a política base obrigatória da organização.

As políticas criadas com políticas pai não vazias herdam todas as coleções de regras da política pai. As coleções de regras de rede herdadas de uma política pai sempre são priorizadas acima das coleções de regras de rede definidas como parte de uma nova política. A mesma lógica também se aplica às coleções de regras de aplicativo. No entanto, as coleções de regras de rede são sempre processadas antes de coleções de regras de aplicativo, independentemente da herança.

O modo de inteligência contra ameaças também é herdado da política pai. Você pode definir o modo de inteligência contra ameaças para um valor diferente a fim de substituir esse comportamento, mas não pode desativá-lo. Só é possível substituí-lo por um valor mais estrito. Por exemplo, se a política pai estiver definida como **Somente alerta**, você poderá configurar essa política local para **Alertar e recusar**.

As coleções de regras de NAT não são herdadas porque são específicas a um determinado firewall.

Com a herança, todas as alterações na política pai são aplicadas automaticamente a políticas filho de firewall associadas.

## <a name="traditional-rules-and-policies"></a>Regras e políticas tradicionais

O Firewall do Azure dá suporte a regras e políticas tradicionais. A seguinte tabela compara políticas e regras:


|         |Política  |Regras  |
|---------|---------|---------|
|Contém     |Configurações de inteligência contra ameaças, rede, regras de aplicativo e NAT|Regras de NAT, de rede e de aplicativo |
|Protege     |Hubs virtuais e redes virtuais|Somente Redes Virtuais|
|Experiência do Portal     |Gerenciamento central usando o Gerenciador de Firewall|Experiência de firewall autônomo|
|Suporte a vários firewalls     |A política de firewall é um recurso separado que pode ser usado em firewalls|Exportar e importar regras manualmente ou usar soluções de gerenciamento de terceiros |
|Preços     |Cobrado com base na associação de firewall. Veja [Preço](#pricing).|Grátis|
|Mecanismos de implantação compatíveis     |Portal, API REST, modelos, Azure PowerShell e CLI|Portal, API REST, modelos, PowerShell e CLI. |
|Status de liberação     |Visualização pública|Disponibilidade geral|

## <a name="pricing"></a>Preços

As políticas são cobradas com base nas associações de firewall. Uma política com zero ou uma associação de firewall é gratuita. Uma política com várias associações de firewall é cobrada segundo uma taxa fixa. Para obter mais informações, confira [Preços do Gerenciador de Firewall do Azure](https://azure.microsoft.com/pricing/details/firewall-manager/).

## <a name="next-steps"></a>Próximas etapas

Para saber como implantar um Firewall do Azure, confira [Tutorial: Proteja sua rede na nuvem com a versão prévia do Gerenciador de Firewall do Azure usando o portal do Azure](secure-cloud-network.md).
