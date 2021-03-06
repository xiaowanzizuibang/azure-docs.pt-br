---
title: Criar um cluster particular do serviço kubernetes do Azure
description: Saiba como criar um cluster do AKS (serviço de kubernetes do Azure) privado
services: container-service
ms.topic: article
ms.date: 2/21/2020
ms.openlocfilehash: 87f52c5a749b531e5b0656e0b30ff0fe9c1a57bf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80398063"
---
# <a name="create-a-private-azure-kubernetes-service-cluster"></a>Criar um cluster particular do serviço kubernetes do Azure

Em um cluster privado, o plano de controle ou o servidor de API tem endereços IP internos que são definidos no documento [alocação de endereço RFC1918 para Internet privada](https://tools.ietf.org/html/rfc1918) . Usando um cluster privado, você pode garantir que o tráfego de rede entre o servidor de API e os pools de nós permaneça apenas na rede privada.

O plano de controle ou o servidor de API está em uma assinatura do Azure gerenciada pelo AKS (serviço de kubernetes do Azure). O cluster ou o pool de nós de um cliente está na assinatura do cliente. O servidor e o cluster ou pool de nós podem se comunicar entre si por meio do [serviço de vínculo privado do Azure][private-link-service] na rede virtual do servidor de API e um ponto de extremidade privado exposto na sub-rede do cluster AKs do cliente.

## <a name="prerequisites"></a>Pré-requisitos

* O CLI do Azure versão 2.2.0 ou posterior

## <a name="create-a-private-aks-cluster"></a>Criar um cluster AKS privado

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos ou use um grupo de recursos existente para o cluster AKS.

```azurecli-interactive
az group create -l westus -n MyResourceGroup
```

### <a name="default-basic-networking"></a>Rede básica padrão 

```azurecli-interactive
az aks create -n <private-cluster-name> -g <private-cluster-resource-group> --load-balancer-sku standard --enable-private-cluster  
```
Onde *--Enable-Private-cluster* é um sinalizador obrigatório para um cluster privado. 

### <a name="advanced-networking"></a>Rede avançada  

```azurecli-interactive
az aks create \
    --resource-group <private-cluster-resource-group> \
    --name <private-cluster-name> \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 
```
Onde *--Enable-Private-cluster* é um sinalizador obrigatório para um cluster privado. 

> [!NOTE]
> Se o endereço de ponte do Docker (172.17.0.1/16) conflitar com o CIDR da sub-rede, altere o endereço da ponte do Docker adequadamente.

## <a name="options-for-connecting-to-the-private-cluster"></a>Opções para se conectar ao cluster privado

O ponto de extremidade do servidor de API não tem nenhum endereço IP público. Para gerenciar o servidor de API, será necessário usar uma VM que tenha acesso à VNet (rede virtual) do Azure do cluster AKS. Há várias opções para estabelecer a conectividade de rede com o cluster privado.

* Crie uma VM na mesma VNet (rede virtual) do Azure que o cluster AKS.
* Use uma VM em uma rede separada e configure o [emparelhamento de rede virtual][virtual-network-peering].  Consulte a seção abaixo para obter mais informações sobre essa opção.
* Use uma conexão [VPN ou de rota expressa][express-route-or-VPN] .

Criar uma VM na mesma VNET que o cluster AKS é a opção mais fácil.  A rota expressa e as VPNs adicionam custos e exigem complexidade adicional de rede.  O emparelhamento de rede virtual exige que você planeje os intervalos de CIDR de rede para garantir que não haja intervalos sobrepostos.

## <a name="virtual-network-peering"></a>Emparelhamento de rede virtual

Conforme mencionado, o emparelhamento VNet é uma maneira de acessar seu cluster privado. Para usar o emparelhamento VNet, você precisa configurar um vínculo entre a rede virtual e a zona DNS privada.
    
1. Vá para o MC_ * grupo de recursos no portal do Azure.  
2. Selecione a zona DNS privada.   
3. No painel esquerdo, selecione o link **rede virtual** .  
4. Crie um novo link para adicionar a rede virtual da VM à zona DNS privada. Leva alguns minutos para que o link da zona DNS fique disponível.  
5. Volte para o grupo de recursos MC_ * no portal do Azure.  
6. No painel direito, selecione a rede virtual. O nome da rede virtual está no formato *AKs-vnet-\**.  
7. No painel esquerdo, selecione **emparelhamentos**.  
8. Selecione **Adicionar**, adicione a rede virtual da VM e, em seguida, crie o emparelhamento.  
9. Vá para a rede virtual onde você tem a VM, selecione **emparelhamentos**, selecione a rede virtual AKs e, em seguida, crie o emparelhamento. Se os intervalos de endereços na rede virtual AKS e o conflito de rede virtual da VM, o emparelhamento falhará. Para obter mais informações, consulte [emparelhamento de rede virtual][virtual-network-peering].

## <a name="hub-and-spoke-with-custom-dns"></a>Hub e spoke com DNS personalizado

[Arquiteturas de Hub e spoke](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) normalmente são usadas para implantar redes no Azure. Em muitas dessas implantações, as configurações de DNS no spoke VNets são configuradas para fazer referência a um encaminhador DNS central para permitir a resolução de DNS local e baseada no Azure. Ao implantar um cluster AKS em um ambiente de rede como esse, há algumas considerações especiais que devem ser levadas em conta.

![Hub de cluster privado e spoke](media/private-clusters/aks-private-hub-spoke.png)

1. Por padrão, quando um cluster privado é provisionado, um ponto de extremidade privado (1) e uma zona DNS privada (2) são criados no grupo de recursos gerenciados do cluster. O cluster usa um registro A na zona privada para resolver o IP do ponto de extremidade privado para comunicação com o servidor de API.

2. A zona DNS privada é vinculada somente à VNet à qual os nós de cluster estão anexados (3). Isso significa que o ponto de extremidade privado só pode ser resolvido por hosts nessa VNet vinculada. Em cenários em que nenhum DNS personalizado está configurado na VNet (padrão), isso funciona sem emitir como hosts Point em 168.63.129.16 para DNS que pode resolver registros na zona DNS privada devido ao link.

3. Em cenários em que a VNet que contém o cluster tem configurações de DNS personalizadas (4), a implantação de cluster falhará, a menos que a zona DNS privada esteja vinculada à VNet que contenha os resolvedores de DNS personalizados (5). Esse link pode ser criado manualmente depois que a zona privada é criada durante o provisionamento do cluster ou via automação na detecção da criação da zona usando Azure Policy ou outros mecanismos de implantação baseados em eventos (por exemplo, grade de eventos do Azure e Azure Functions).

## <a name="dependencies"></a>Dependências  

* O serviço de vínculo privado tem suporte somente no Azure Load Balancer padrão. Não há suporte para Azure Load Balancer básica.  
* Para usar um servidor DNS personalizado, adicione o 168.63.129.16 de IP do DNS do Azure como o servidor DNS upstream no servidor DNS personalizado.

## <a name="limitations"></a>Limitações 
* Os intervalos autorizados por IP não podem ser aplicados ao ponto de extremidade do servidor de API privada, eles se aplicam somente ao servidor de API pública
* Atualmente, há suporte para Zonas de Disponibilidade em determinadas regiões, consulte o início deste documento 
* As [limitações do serviço de vínculo privado do Azure][private-link-service] se aplicam a clusters privados, pontos de extremidade privados do Azure e pontos de extremidade de serviço de rede virtual, que atualmente não têm suporte na mesma rede virtual.
* Não há suporte para nós virtuais em um cluster privado para girar ACI (instâncias de contêiner do Azure particulares) em uma rede virtual do Azure privada
* Não há suporte para a integração do Azure DevOps pronta para uso com clusters privados
* Para clientes que precisam habilitar o registro de contêiner do Azure para trabalhar com AKS particulares, a rede virtual do registro de contêiner deve ser emparelhada com a rede virtual do cluster do agente.
* Não há suporte atual para Azure Dev Spaces
* Não há suporte para converter clusters AKS existentes em clusters privados
* Excluir ou modificar o ponto de extremidade privado na sub-rede do cliente fará com que o cluster pare de funcionar. 
* No momento, não há suporte para Azure Monitor para contêineres de dados dinâmicos.


<!-- LINKS - internal -->
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[private-link-service]: /azure/private-link/private-link-service-overview
[virtual-network-peering]: ../virtual-network/virtual-network-peering-overview.md
[azure-bastion]: ../bastion/bastion-create-host-portal.md
[express-route-or-vpn]: ../expressroute/expressroute-about-virtual-network-gateways.md

