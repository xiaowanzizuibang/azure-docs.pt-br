---
title: 'Modificar configurações de endereço IP do gateway: PowerShell'
description: Este artigo mostra o passo a passo da alteração dos prefixos de endereço IP do seu gateway de rede local usando o PowerShell
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: dbaef6e0c81a9230b24aa1e85e7fdc421444047d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75863988"
---
# <a name="modify-local-network-gateway-settings-using-powershell"></a>Modificar as configurações de gateway de rede local usando o PowerShell

Às vezes, as configurações do seu gateway de rede local AddressPrefix ou GatewayIPAddress mudam. Este artigo mostra como modificar as configurações de gateway de rede local. Também é possível modificar essas configurações usando um método diferente, selecionando uma opção diferente da lista a seguir:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [CLI do Azure](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before-you-begin"></a><a name="before"></a>Antes de começar

Instale a versão mais recente dos cmdlets do PowerShell do Azure Resource Manager. Consulte [Como instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs) para obter mais informações sobre como instalar os cmdlets do PowerShell.

## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>Modificar os prefixos de endereço IP

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>Modificar o endereço IP do gateway

[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Próximas etapas

Você pode verificar a conexão de gateway. Confira [Verificar uma conexão de gateway](vpn-gateway-verify-connection-resource-manager.md).