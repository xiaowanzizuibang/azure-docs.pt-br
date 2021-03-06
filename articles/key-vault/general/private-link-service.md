---
title: Integrar ao serviço de Link Privado do Azure
description: Saiba como integrar o Azure Key Vault ao Serviço de Link Privado do Azure
author: ShaneBala-keyvault
ms.author: sudbalas
ms.date: 03/08/2020
ms.service: key-vault
ms.subservice: general
ms.topic: quickstart
ms.openlocfilehash: 678e91126c04d5b299d9234a1602580260c5aee6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81421560"
---
# <a name="integrate-key-vault-with-azure-private-link"></a>Integrar o Key Vault ao Link Privado do Azure

O Serviço de Link Privado do Azure permite acessar os Serviços do Azure (por exemplo, o Azure Key Vault, o Armazenamento do Azure e o Azure Cosmos DB) e serviços de parceiros/clientes hospedados no Azure em um Ponto de Extremidade Privado em sua rede virtual.

Um Ponto de Extremidade Privado do Azure é um adaptador de rede que conecta você de maneira privada e segura a um serviço com tecnologia do Link Privado do Azure. O ponto de extremidade privado usa um endereço IP privado de sua VNet, colocando efetivamente em sua VNet. Todo o tráfego para o serviço pode ser roteado por meio do ponto de extremidade privado; assim, nenhum gateway, nenhum dispositivo NAT, nenhuma conexão ExpressRoute ou VPN e nenhum endereço IP público é necessário. O tráfego entre a rede virtual e o serviço percorre a rede de backbone da Microsoft, eliminando a exposição da Internet pública. Você pode se conectar a uma instância de um recurso do Azure, fornecendo o nível mais alto de granularidade no controle de acesso.

Para obter mais informações, confira [O que é o Link Privado do Azure?](../../private-link/private-link-overview.md)

## <a name="prerequisites"></a>Pré-requisitos

Para integrar um cofre de chaves com o Link Privado do Azure, você precisará do seguinte:

- Um cofre de chaves.
- Uma rede virtual do Azure.
- Uma sub-rede na rede virtual.
- Permissões de proprietário ou colaborador para o cofre de chaves e a rede virtual.

Seu ponto de extremidade privado e a rede virtual devem estar na mesma região. Quando você selecionar uma região para o ponto de extremidade privado usando o portal, ele filtrará automaticamente apenas as redes virtuais que estiverem nessa região. Seu cofre de chaves pode estar em uma região diferente.

Seu ponto de extremidade privado usa um endereço IP privado em sua rede virtual.

## <a name="establish-a-private-link-connection-to-key-vault-using-the-azure-portal"></a>Estabelecer uma conexão de link privado com o Key Vault usando o portal do Azure 

Primeiro, crie uma rede virtual seguindo as etapas em [Criar uma rede virtual usando o portal do Azure](../../virtual-network/quick-create-portal.md)

Você pode criar um cofre de chaves ou estabelecer uma conexão de link privado com um cofre de chaves existente.

### <a name="create-a-new-key-vault-and-establish-a-private-link-connection"></a>Criar um cofre de chaves e estabelecer uma conexão de link privado

Você pode criar um cofre de chaves seguindo as etapas em [Definir e recuperar um segredo do Azure Key Vault usando o portal do Azure](../secrets/quick-create-portal.md)

Após configurar os conceitos básicos do cofre de chaves, selecione a guia Rede e siga estas etapas:

1. Selecione o botão de opção do Ponto de Extremidade Privado na guia Rede.
1. Clique no botão “+ Adicionar” para adicionar um ponto de extremidade privado.

    ![Imagem](../media/private-link-service-1.png)
 
1. No campo “Localização” da Folha do Ponto de Extremidade Privado, selecione a região na qual sua rede virtual está localizada. 
1. No campo “Nome”, crie um nome descritivo que permitirá identificar esse ponto de extremidade privado. 
1. Selecione a rede virtual e a sub-rede na qual você deseja que esse ponto de extremidade privado seja criado no menu suspenso. 
1. Deixe a opção "integrar-se ao DNS de zona privada" inalterada.  
1. Selecione "Ok".

    ![Imagem](../media/private-link-service-8.png)
 
Agora você poderá ver o ponto de extremidade privado configurado. Agora você tem a opção de excluir e editar esse ponto de extremidade privado. Selecione o botão “Examinar + Criar” e crie o cofre de chaves. Levará entre 5 e 10 minutos para a implantação ser concluída. 

### <a name="establish-a-private-link-connection-to-an-existing-key-vault"></a>Estabelecer uma conexão de link privado com o cofre de chaves existente

Se você já tem um cofre de chaves, pode criar uma conexão de link privado seguindo estas etapas:

1. Entre no portal do Azure. 
1. Na barra de pesquisa, digite “cofres de chaves”
1. Selecione o cofre de chaves na lista à qual você deseja adicionar um ponto de extremidade privado.
1. Selecione a guia “Rede” em Configurações
1. Selecione a guia Conexões de ponto de extremidade privado na parte superior da página
1. Selecione o botão “+ Ponto de Extremidade Privado” na parte superior da página.

    ![Imagem](../media/private-link-service-3.png) ![Imagem](../media/private-link-service-4.png)

Você pode optar por criar um ponto de extremidade privado para qualquer recurso do Azure ao usar esta folha. Você pode usar os menus suspensos para selecionar um tipo de recurso e selecionar um recurso em seu diretório ou pode se conectar a qualquer recurso do Azure usando uma ID do recurso. Deixe a opção "integrar-se ao DNS de zona privada" inalterada.  

![Image](../media/private-link-service-3.png)
![Image](../media/private-link-service-4.png)

## <a name="establish-a-private-link-connection-to-key-vault-using-cli"></a>Estabelecer uma conexão de link privado com o Key Vault usando a CLI

### <a name="login-to-azure-cli"></a>Fazer logon na CLI do Azure
```console
az login 
```
### <a name="select-your-azure-subscription"></a>Selecionar sua Assinatura do Azure 
```console
az account set --subscription {AZURE SUBSCRIPTION ID}
```
### <a name="create-a-new-resource-group"></a>Criar um grupo de recursos 
```console
az group create -n {RG} -l {AZURE REGION}
```
### <a name="register-microsoftkeyvault-as-a-provider"></a>Registrar Microsoft.KeyVault como um provedor 
```console
az provider register -n Microsoft.KeyVault
```
### <a name="create-a-new-key-vault"></a>Criar um Key Vault
```console
az keyvault create --name {KEY VAULT NAME} --resource-group {RG} --location {AZURE REGION}
```
### <a name="turn-on-key-vault-firewall"></a>Ativar o firewall do Key Vault
```console
az keyvault update --name {KEY VAULT NAME} --resource-group {RG} --location {AZURE REGION} --default-action deny
```
### <a name="create-a-virtual-network"></a>Criar uma rede virtual
```console
az network vnet create --resource-group {RG} --name {vNet NAME} --location {AZURE REGION}
```
### <a name="add-a-subnet"></a>Adicionar uma sub-rede
```console
az network vnet subnet create --resource-group {RG} --vnet-name {vNet NAME} --name {subnet NAME} --address-prefixes {addressPrefix}
```
### <a name="disable-virtual-network-policies"></a>Desabilitar Políticas de Rede Virtual 
```console
az network vnet subnet update --name {subnet NAME} --resource-group {RG} --vnet-name {vNet NAME} --disable-private-endpoint-network-policies true
```
### <a name="add-a-private-dns-zone"></a>Adicionar uma Zona DNS Privado 
```console
az network private-dns zone create --resource-group {RG} --name privatelink.vaultcore.azure.net
```
### <a name="link-private-dns-zone-to-virtual-network"></a>Vincular Zona DNS Privado à Rede Virtual 
```console
az network private-dns link vnet create --resoruce-group {RG} --virtual-network {vNet NAME} --zone-name privatelink.vaultcore.azure.net --name {dnsZoneLinkName} --registration-enabled true
```
### <a name="create-a-private-endpoint-automatically-approve"></a>Criar um Ponto de Extremidade Privado (Aprovar Automaticamente) 
```console
az network private-endpoint create --resource-group {RG} --vnet-name {vNet NAME} --subnet {subnet NAME} --name {Private Endpoint Name}  --private-connection-resource-id "/subscriptions/{AZURE SUBSCRIPTION ID}/resourceGroups/{RG}/providers/Microsoft.KeyVault/vaults/ {KEY VAULT NAME}" --group-ids vault --connection-name {Private Link Connection Name} --location {AZURE REGION}
```
### <a name="create-a-private-endpoint-manually-request-approval"></a>Criar um Ponto de Extremidade Privado (Solicitar Aprovação Manualmente) 
```console
az network private-endpoint create --resource-group {RG} --vnet-name {vNet NAME} --subnet {subnet NAME} --name {Private Endpoint Name}  --private-connection-resource-id "/subscriptions/{AZURE SUBSCRIPTION ID}/resourceGroups/{RG}/providers/Microsoft.KeyVault/vaults/ {KEY VAULT NAME}" --group-ids vault --connection-name {Private Link Connection Name} --location {AZURE REGION} --manual-request
```
### <a name="show-connection-status"></a>Mostrar Status de Conexão 
```console
az network private-endpoint show --resource-group {RG} --name {Private Endpoint Name}
```
## <a name="manage-private-link-connection"></a>Gerenciar conexão de link privado

Quando você cria um ponto de extremidade privado, a conexão deve ser aprovada. Se o recurso para o qual você está criando um ponto de extremidade privado estiver em seu diretório, você poderá aprovar a solicitação de conexão desde que tenha permissões suficientes; se você estiver se conectando a um recurso do Azure em outro diretório, deverá aguardar até que o proprietário desse recurso aprove sua solicitação de conexão.

Há quatro estados de provisionamento:

| Ação de executar do serviço | Estado de ponto de extremidade privado do consumidor do serviço | Descrição |
|--|--|--|
| Nenhum | Pendente | A conexão é criada manualmente e está pendente de aprovação do proprietário do recurso do Link Privado. |
| Aprovar | Aprovado | A conexão foi aprovada automaticamente ou manualmente e está pronta para ser usada. |
| Rejeitar | Rejeitado | A conexão foi rejeitada pelo proprietário do recurso do link privado. |
| Remover | Desconectado | A conexão foi removida pelo proprietário do recurso do link privado, o ponto de extremidade privado se torna informativo e deve ser excluído para limpeza. |
 
###  <a name="how-to-manage-a-private-endpoint-connection-to-key-vault-using-the-azure-portal"></a>Como gerenciar uma conexão de ponto de extremidade privado com o Key Vault usando o portal do Azure 

1. Faça logon no Portal do Azure.
1. Na barra de pesquisa, digite “cofres de chaves”
1. Selecione o cofre de chaves que você deseja gerenciar.
1. Selecione a guia “Rede”.
1. Se houver conexões pendentes, você verá uma lista de conexão com “Pendente” no estado de provisionamento. 
1. Selecione o ponto de extremidade privado que você deseja aprovar
1. Selecione o botão aprovar.
1. Se houver conexões de ponto de extremidade privado que você deseja rejeitar, seja uma solicitação pendente ou uma conexão existente, selecione a conexão e clique no botão “Rejeitar”.

    ![Imagem](../media/private-link-service-7.png)

##  <a name="how-to-manage-a-private-endpoint-connection-to-key-vault-using-azure-cli"></a>Como gerenciar uma conexão de ponto de extremidade privado com o Key Vault usando a CLI do Azure

### <a name="approve-a-private-link-connection-request"></a>Aprovar uma Solicitação de Conexão de Link Privado
```console
az keyvault private-endpoint-connection approve --approval-description {"OPTIONAL DESCRIPTION"} --resource-group {RG} --vault-name {KEY VAULT NAME} –name {PRIVATE LINK CONNECTION NAME}
```

### <a name="deny-a-private-link-connection-request"></a>Negar uma Solicitação de Conexão de Link Privado
```console
az keyvault private-endpoint-connection reject --rejection-description {"OPTIONAL DESCRIPTION"} --resource-group {RG} --vault-name {KEY VAULT NAME} –name {PRIVATE LINK CONNECTION NAME}
```

### <a name="delete-a-private-link-connection-request"></a>Excluir uma Solicitação de Conexão de Link Privado
```console
az keyvault private-endpoint-connection delete --resource-group {RG} --vault-name {KEY VAULT NAME} --name {PRIVATE LINK CONNECTION NAME}
```

## <a name="validate-that-the-private-link-connection-works"></a>Validar se a conexão de link privado funciona

Você deve validar se os recursos dentro da mesma sub-rede do recurso de ponto de extremidade privado estão se conectando ao cofre de chaves por um endereço IP privado e se eles têm a integração de zona DNS privada correta.

Primeiro, crie uma máquina virtual seguindo as etapas em [Criar uma máquina virtual do Windows no portal do Azure](../../virtual-machines/windows/quick-create-portal.md)

Na guia “Rede”:

1. especifique a Rede virtual e a Sub-rede. Você pode criar uma rede virtual ou selecionar uma existente. Se você estiver selecionando uma existente, verifique se a região é correspondente.
1. Especifique um recurso de IP Público.
1. No "grupo de segurança de rede da NIC", selecione "Nenhum".
1. No “Balanceamento de carga”, selecione “Não”.

Abra a linha de comando e execute o seguinte comando:

```console
nslookup <your-key-vault-name>.vault.azure.net
```

Se você executar o comando de pesquisa para resolver o endereço IP de um cofre de chaves por meio de um ponto de extremidade público, verá um resultado com esta aparência:

```console
c:\ >nslookup <your-key-vault-name>.vault.azure.net

Non-authoritative answer:
Name:    
Address:  (public IP address)
Aliases:  <your-key-vault-name>.vault.azure.net
```

Se você executar o comando de pesquisa para resolver o endereço IP de um cofre de chaves por meio de um ponto de extremidade privado, verá um resultado com esta aparência:

```console
c:\ >nslookup your_vault_name.vault.azure.net

Non-authoritative answer:
Name:    
Address:  10.1.0.5 (private IP address)
Aliases:  <your-key-vault-name>.vault.azure.net
          <your-key-vault-name>.privatelink.vaultcore.azure.net
```

## <a name="limitations-and-design-considerations"></a>Limitações e considerações de design

**Preço**: Para obter informações sobre preço, confira [Preço do Link Privado do Azure](https://azure.microsoft.com/pricing/details/private-link/).

**Limitações**:  O ponto de extremidade privado para o Azure Key Vault só está disponível nas regiões públicas do Azure.

**Número máximo de Pontos de Extremidade Privados por Key Vault**: 64.

**Número máximo de Key Vaults com Pontos de Extremidade Privados por Assinatura**: 64.

Para saber mais, confira [Serviço de Link Privado do Azure: Limitações](../../private-link/private-link-service-overview.md#limitations)

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre o [Link Privado do Azure](../../private-link/private-link-service-overview.md)
- Saiba mais sobre o [Azure Key Vault](overview.md)
