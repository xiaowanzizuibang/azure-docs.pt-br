---
title: Túnel forçado do firewall do Azure
description: Você pode configurar o túnel forçado para rotear o tráfego vinculado à Internet para um firewall adicional ou solução de virtualização de rede para processamento adicional.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 02/24/2020
ms.author: victorh
ms.openlocfilehash: e51f6de370a5340082f64a0ca15c61583f75962b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77597262"
---
# <a name="azure-firewall-forced-tunneling-preview"></a>Túnel forçado do firewall do Azure (visualização)

Você pode configurar o Firewall do Azure para rotear todo o tráfego vinculado à Internet para um próximo salto designado em vez de ir diretamente para a Internet. Por exemplo, você pode ter um firewall de borda local ou outra solução de virtualização de rede (NVA) para processar o tráfego de rede antes que ele seja passado para a Internet.

> [!IMPORTANT]
> O túnel forçado do firewall do Azure está atualmente em visualização pública.
>
> Essa versão prévia pública é fornecida sem um SLA e não deve ser usada para cargas de trabalho de produção. Talvez alguns recursos não tenham suporte, tenham recursos restritos ou não estejam disponíveis em todos os locais do Azure. Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Por padrão, o túnel forçado não é permitido no firewall do Azure para garantir que todas as suas dependências de saída do Azure sejam atendidas. As configurações de UDR (rota definida pelo usuário) no *AzureFirewallSubnet* que têm uma rota padrão que não vai diretamente para a Internet estão desabilitadas.

## <a name="forced-tunneling-configuration"></a>Configuração de túnel forçado

Para dar suporte ao túnel forçado, o tráfego de gerenciamento de serviços é separado do tráfego do cliente. Uma sub-rede adicional dedicada denominada *AzureFirewallManagementSubnet* (tamanho mínimo da sub-rede/26) é necessária com seu próprio endereço IP público associado. A única rota permitida nessa sub-rede é uma rota padrão para a Internet, e a propagação de rota BGP deve ser desabilitada.

Se você tiver uma rota padrão anunciada via BGP para forçar o tráfego para o local, deverá criar o *AzureFirewallSubnet* e o *AzureFirewallManagementSubnet* antes de implantar o firewall e ter um UDR com uma rota padrão para a Internet e a **propagação de rota do gateway de rede virtual** desabilitada.

Nessa configuração, o *AzureFirewallSubnet* agora pode incluir rotas para qualquer firewall local ou NVA para processar o tráfego antes que ele seja passado para a Internet. Você também pode publicar essas rotas via BGP para *AzureFirewallSubnet* se a **propagação de rota de gateway de rede virtual** estiver habilitada nesta sub-rede.

Por exemplo, você pode criar uma rota padrão no *AzureFirewallSubnet* com seu gateway de VPN como o próximo salto para chegar ao dispositivo local. Ou você pode habilitar a **propagação de rota de gateway de rede virtual** para obter as rotas apropriadas para a rede local.

![Propagação de rota do gateway de rede virtual](media/forced-tunneling/route-propagation.png)

Depois de configurar o Firewall do Azure para dar suporte ao túnel forçado, não é possível desfazer a configuração. Se você remover todas as outras configurações de IP em seu firewall, a configuração de IP de gerenciamento também será removida e o firewall será desalocado. O endereço IP público atribuído à configuração de IP de gerenciamento não pode ser removido, mas você pode atribuir um endereço IP público diferente.

## <a name="next-steps"></a>Próximas etapas

- [Tutorial: Implantar e configurar o Firewall do Azure em uma rede híbrida usando o portal do Azure](tutorial-hybrid-portal.md)