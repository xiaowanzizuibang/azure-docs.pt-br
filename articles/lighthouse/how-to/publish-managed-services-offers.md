---
title: Publicar uma oferta de serviço gerenciado no Azure Marketplace
description: Saiba como publicar uma oferta de serviço gerenciado que integra os clientes ao gerenciamento de recursos delegado do Azure.
ms.date: 04/24/2020
ms.topic: conceptual
ms.openlocfilehash: 19c4d1a4bd0ffd7c0162cbf7f20c49a5b219b9bc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146733"
---
# <a name="publish-a-managed-service-offer-to-azure-marketplace"></a>Publicar uma oferta de serviço gerenciado no Azure Marketplace

> [!IMPORTANT]
> A partir de 13 de abril de 2020, começaremos a mover o gerenciamento de suas ofertas de serviço gerenciado para o Partner Center. Após a migração, você criará e gerenciará suas ofertas no Partner Center. Siga as instruções em [criar uma nova oferta de serviço gerenciado](../../marketplace/partner-center-portal/create-new-managed-service-offer.md) para gerenciar suas ofertas migradas.

Neste artigo, você aprenderá a publicar uma oferta de serviço gerenciado pública ou privada para o [Azure Marketplace](https://azuremarketplace.microsoft.com) usando o [portal do Cloud Partner](https://cloudpartner.azure.com/). Os clientes que comprarem a oferta serão capazes de carregar assinaturas e grupos de recursos para o [Gerenciamento de recursos delegado do Azure](../concepts/azure-delegated-resource-management.md).

## <a name="publishing-requirements"></a>Publicação de requisitos

Você precisa ter uma conta válida [no Partner Center](../../marketplace/partner-center-portal/create-account.md) para criar e publicar ofertas. Se você ainda não tiver uma conta, o [processo de inscrição](https://aka.ms/joinmarketplace) o conduzirá pelas etapas de criação de uma conta no Partner Center e registro no programa do Marketplace comercial.

De acordo com os [requisitos de certificação da oferta de serviço gerenciado](https://docs.microsoft.com/legal/marketplace/certification-policies#7004-business-requirements), você deve ter um [nível de competência de plataforma de nuvem prata ou ouro](https://docs.microsoft.com/partner-center/learn-about-competencies) ou ser um [Azure expert MSP](https://partner.microsoft.com/membership/azure-expert-msp) para publicar uma oferta de serviço gerenciado.

Sua ID do MPN (Microsoft Partner Network) será [associada automaticamente](../../billing/billing-partner-admin-link-started.md) às ofertas que você publicar para controlar seu impacto na participação dos clientes.

> [!NOTE]
> Se não quiser publicar uma oferta no Azure Marketplace, você poderá integrar os clientes manualmente usando modelos do Azure Resource Manager. Para saber mais, confira [Integrar um cliente no gerenciamento de recursos delegados do Azure](onboard-customer.md).

A publicação de uma oferta de serviço gerenciado é semelhante à publicação de qualquer outro tipo de oferta no Azure Marketplace. Para saber mais sobre o processo de publicação geral, consulte o [Guia de publicação do Azure Marketplace e do AppSource](../../marketplace/marketplace-publishers-guide.md). Você também deve examinar as [políticas de certificação do mercado comercial](https://docs.microsoft.com/legal/marketplace/certification-policies), especialmente a seção [Serviços Gerenciados](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services).

Depois que um cliente adiciona sua oferta, ele poderá delegar uma ou mais assinaturas ou grupos de recursos, que serão [integrados ao gerenciamento de recursos delegado do Azure](#the-customer-onboarding-process).

> [!IMPORTANT]
> Cada plano em uma oferta de serviço gerenciado inclui uma seção **detalhes do manifesto** , em que você define as entidades Azure Active Directory (Azure AD) em seu locatário que terão acesso aos grupos de recursos delegados e/ou assinaturas para clientes que compram esse plano. É importante estar ciente de que qualquer grupo (ou entidade de serviço ou usuário) que você incluir terá as mesmas permissões para cada cliente que comprar o plano. Para atribuir grupos diferentes para trabalhar com cada cliente, você precisará publicar um [plano privado](../../marketplace/private-offers.md) separado que seja exclusivo para cada cliente.

## <a name="create-your-offer"></a>Criar sua oferta

1. Entre no [portal do Cloud Partner](https://cloudpartner.azure.com/).
2. No menu de navegação à esquerda, selecione **Nova oferta** e selecione **Serviços gerenciados**.
3. Você verá uma seção do **Editor** para sua oferta com quatro partes a serem preenchidas: **configurações da oferta**, **planos**, **Marketplace**e **suporte**. Continue lendo para obter diretrizes sobre como concluir essas seções.

### <a name="enter-offer-settings"></a>Inserir configurações da oferta

Na seção **Configurações da oferta**, forneça o seguinte:

|Campo  |Descrição  |
|---------|---------|
|**ID da oferta**     | Um identificador exclusivo para a oferta (dentro do seu perfil de fornecedor). Essa ID pode conter apenas caracteres alfanuméricos em minúsculas, traços e sublinhados, com no máximo 50 caracteres. Tenha em mente que a ID da oferta pode ficar visível para clientes em locais como URLs de produtos e relatórios de cobrança. Depois de publicar a oferta, você não poderá alterar esse valor.        |
|**ID do Editor**     | A ID do fornecedor que será associada à oferta. Se você tem mais de uma ID de fornecedor, pode selecionar aquela que deseja usar para essa oferta.       |
|**Nome**     | O nome da oferta (com até 50 caracteres) que os clientes verão no Azure Marketplace e no portal do Azure. Use um nome de marca reconhecível que os clientes compreendam; se você estiver promovendo essa oferta em seu próprio site, use exatamente o mesmo nome aqui.        |

Quando terminar, selecione **Salvar**. Agora você está pronto para passar para a seção **Planos**.

### <a name="create-plans"></a>Criar planos

Cada oferta deve ter um ou mais planos (às vezes chamados de SKUs). Você pode adicionar vários planos para dar suporte a diferentes conjuntos de recursos com preços diferentes ou para personalizar um plano específico para um público seleto de clientes específicos. Os clientes podem ver os planos disponíveis para eles na oferta pai.

Na seção Planos, selecione **Novo plano**. Em seguida, insira uma **ID de Plano**. Essa ID pode conter apenas caracteres alfanuméricos em minúsculas, traços e sublinhados, com no máximo 50 caracteres. A ID do plano pode ficar visível para clientes em locais como URLs de produtos e relatórios de cobrança. Depois de publicar a oferta, você não poderá alterar esse valor.

#### <a name="plan-details"></a>Detalhes do plano

Preencha as seguintes seções em **Detalhes do plano**:

|Campo  |Descrição  |
|---------|---------|
|**Título**     | Nome amigável do plano para exibição. Comprimento máximo de 50 caracteres.        |
|**Resumo**     | Descrição sucinta do plano para exibição abaixo do título. Comprimento máximo de 100 caracteres.        |
|**Descrição**     | Texto de descrição que fornece uma explicação mais detalhada do plano.         |
|**Modelo de cobrança**     | Há dois modelos de cobrança mostrados aqui, mas você deve escolher **Traga sua própria licença** para ofertas de serviços gerenciados. Isso significa que você cobrará de seus clientes diretamente os custos relacionados a essa oferta, e a Microsoft não cobrará nenhum valor por você.   |
|**Este é um plano privado?**     | Indica se a SKU é pública ou privada. O padrão é **Não** (público). Se você deixar essa seleção, seu plano não será restrito a clientes específicos (ou a determinado número de clientes); depois de publicar um plano público, você não poderá alterá-lo posteriormente para privado. Para disponibilizar esse plano somente para clientes específicos, selecione **Sim**. Ao fazer isso, você precisará identificar os clientes fornecendo suas IDs de assinatura. Elas podem ser inseridas uma a uma (até 10 assinaturas) ou pelo upload de um arquivo .csv (até 20.000 assinaturas). Inclua suas próprias assinaturas aqui para que você possa testar e validar a oferta. Para saber mais, confira [SKUs Privados e Planos](../../marketplace/cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md).  |

> [!IMPORTANT]
> Depois que um plano tiver sido publicado como público, você não poderá alterá-lo para privado. Para controlar quais clientes podem aceitar sua oferta e delegar recursos, use um plano privado. Com um plano público, não é possível restringir a disponibilidade para determinados clientes ou até mesmo para um determinado número de clientes (embora você possa deixar de vender completamente o plano se optar por fazê-lo). Você poderá [remover o acesso a uma delegação](remove-delegation.md) depois que um cliente aceitar uma oferta somente se você tiver incluído uma **autorização** com a **definição de função** definida para atribuição de [registro de serviços gerenciados excluir função](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) quando você publicou a oferta. Você também pode entrar em contato com o cliente e pedir que eles [removam seu acesso](view-manage-service-providers.md#add-or-remove-service-provider-offers).

#### <a name="manifest-details"></a>Detalhes do manifesto

Preencha a seção **Detalhes do Manifesto** do seu plano. Isso criará um manifesto com informações de autorização para gerenciar recursos do cliente. Essas informações são necessárias para habilitar o gerenciamento de recursos delegados do Azure.

> [!NOTE]
> Como observado acima, os usuários e as funções em sua entradas **autorização** serão aplicadas a todos os clientes que comprarem o plano. Se você quiser limitar o acesso a um cliente específico, será necessário publicar um plano privado para uso exclusivo.

Primeiro, forneça uma **Versão** para o manifesto. Use o formato *n.n.n* (por exemplo, 1.2.5).

Em seguida, insira sua **ID de Locatário**. Ela é um GUID associado à ID de locatário do Azure Active Directory da sua organização (ou seja, o locatário no qual você trabalhará para gerenciar os recursos de seus clientes). Se você não tiver isso à mão, poderá encontrá-lo passando o mouse sobre o nome da conta no lado superior direito do portal do Azure ou selecionando o **Mudar diretório**.

Por fim, adicione uma ou mais entradas de **Autorização** ao plano. As autorizações definem as entidades que podem acessar recursos e assinaturas para clientes que adquirem o plano e atribuem funções que concedem níveis específicos de acesso.

> [!TIP]
> Na maioria dos casos, é melhor atribuir permissões a um grupo de usuários ou entidade de serviço do Azure AD, em vez de a uma série de contas de usuário individuais. Assim você pode adicionar ou remover o acesso de usuários individuais sem precisar atualizar e publicar o plano novamente quando os requisitos de acesso forem alterados. Para obter recomendações adicionais, confira [Locatários, funções e usuários em cenários do Azure Lighthouse](../concepts/tenants-users-roles.md).

Para cada **Autorização**, você precisará fornecer o seguinte. Em seguida, você pode selecionar **Nova autorização** quantas vezes forem necessárias para adicionar mais definições de usuários e funções.

- **ID de objeto do Azure ad**: o identificador do Azure AD de um usuário, grupo de usuários ou aplicativo ao qual serão concedidas determinadas permissões (conforme descrito pela definição de função) aos recursos dos seus clientes.
- **Nome de exibição do objeto do Azure ad**: um nome amigável para ajudar o cliente a entender a finalidade dessa autorização. O cliente verá esse nome ao delegar recursos.
- **Definição de função**: selecione uma das funções internas do Azure ad disponíveis na lista. Essa função determinará as permissões que o usuário no campo **ID do Objeto do Azure AD** terá nos recursos dos clientes. Para obter descrições dessas funções, consulte [funções internas](../../role-based-access-control/built-in-roles.md) e [suporte de função para o gerenciamento de recursos delegado do Azure](../concepts/tenants-users-roles.md#role-support-for-azure-delegated-resource-management).
  > [!NOTE]
  > Conforme as novas funções internas aplicáveis forem adicionadas ao Azure, elas ficarão disponíveis aqui, embora possa haver algum atraso antes que elas apareçam.
- **Funções atribuíveis**: isso será necessário somente se você tiver selecionado administrador de acesso de usuário na **definição de função** para essa autorização. Nesse caso, você deve adicionar uma ou mais funções atribuíveis aqui. O usuário no campo **ID de objeto do Azure AD** poderá atribuir essas **Funções atribuíveis** às [identidades gerenciadas](../../active-directory/managed-identities-azure-resources/overview.md), necessárias para [implantar políticas que possam ser corrigidas](deploy-policy-remediation.md). Observe que nenhuma outra permissão normalmente associada à função Administrador de Acesso de Usuário será aplicada a esse usuário. Se você não selecionar uma ou mais funções aqui, seu envio não será aprovado na certificação. (Se você não selecionou administrador de acesso do usuário para a definição de função deste usuário, esse campo não terá efeito.)

> [!TIP]
> Para garantir que você possa [remover o acesso a uma delegação](remove-delegation.md) , se necessário, inclua uma **autorização** com a **definição de função** definida como atribuição de registro de [Serviços gerenciados excluir função](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role). Se essa função não for atribuída, os recursos delegados só poderão ser removidos por um usuário no locatário do cliente.

Depois de concluir as informações, você poderá selecionar o **Novo plano** quantas vezes precisar para criar planos adicionais. Ao terminar, selecione **Salvar** e continue na seção **Marketplace**.

### <a name="provide-marketplace-text-and-images"></a>Fornecer texto e imagens ao Marketplace

A seção **Marketplace** é onde você fornece o texto e as imagens que os clientes verão no Azure Marketplace e no portal do Azure.

Preencha os seguintes campos na seção **Visão geral**:

|Campo  |Descrição  |
|---------|---------|
|**Título**     |  Título da oferta, geralmente o nome longo e formal. Este título será exibido com destaque no mercado. Comprimento máximo de 50 caracteres. Na maioria dos casos, ele deve ser igual ao **Nome** inserido na seção **Configurações da Oferta**.       |
|**Resumo**     | Breve propósito ou função da solução. Isso geralmente é exibido abaixo do título. Comprimento máximo de 100 caracteres.        |
|**Resumo longo**     | Um resumo mais longo da finalidade ou da função da oferta. Comprimento máximo de 256 caracteres.        |
|**Descrição**     | Mais informações sobre sua oferta. É um campo com tamanho máximo de 3000 caracteres e dá suporte à formatação HTML simples. Você deve incluir as palavras "serviço gerenciado" ou "serviços gerenciados" em algum lugar na sua descrição.       |
|**Identificador de marketing**     | Um identificador amigável de URL exclusivo. Esse identificador só pode conter caracteres alfanuméricos minúsculos e traços. Ele será usado em URLs do Marketplace para esta oferta. Por exemplo, se a ID do editor for *contoso* e o identificador de marketing for *sampleApp*, a URL da oferta no Azure Marketplace será `https://azuremarketplace.microsoft.com/marketplace/apps/contoso-sampleApp`.       |
|**Visualizar IDs de assinatura**     | Adicione 1 a 100 identificadores de assinatura. Os clientes associados a essas assinaturas poderão exibir a oferta no Azure Marketplace antes de ser ativada. Sugerimos incluir suas próprias assinaturas aqui para que você possa visualizar como a oferta aparece no Azure Marketplace antes de disponibilizá-la para os clientes.  (As equipes de suporte e engenharia da Microsoft também poderão exibir a oferta durante esse período de versão prévia.)   |
|**Links úteis**     | URLs relacionadas à oferta, como documentação, notas de versão, perguntas frequentes, etc.        |
|**Categorias sugeridas (máximo 5)**     | Uma ou mais categorias (até cinco) que se aplicam à oferta. Essas categorias ajudam os clientes a descobrir a oferta no Azure Marketplace e no portal do Azure.        |

Na seção **Artefatos de Marketing**, você pode fazer upload de logotipos e outros ativos para serem mostrados com a oferta. Opcionalmente, você pode fazer upload de capturas de tela ou links para vídeos que podem ajudar os clientes a entender a oferta.

São necessários quatro tamanhos de logotipo: **Small (40x40)**, **Medium (90x90)**, **Large (115x115)** e **Wide (255x115)**. Siga estas diretrizes para seus logotipos:

- O design do Azure tem uma paleta de cores simples. Limite o número de cores primárias e secundárias em seu logotipo.
- As cores do tema do portal são branca e preta. Não use essas cores como a cor de fundo de seu logotipo. Use uma cor que destaque seu logotipo no portal. É recomendável usar cores primárias simples.
- Se você usar um plano de fundo transparente, verifique se o logotipo e o texto não estão em branco, preto ou azul.
- A aparência de seu logotipo deve ser simples e evitar gradientes. Não use um fundo gradiente no logotipo.
- Não coloque texto no logotipo, nem mesmo o nome ou marca da sua empresa.
- Verifique se o logotipo não está esticado.

O logotipo **Hero** (815 x 290) é opcional, mas recomendado. Se você incluir um logotipo Hero, siga estas diretrizes:

- Não inclua nenhum texto no logotipo Hero e lembre-se de deixar 415 pixels de espaço vazio no lado direito do logotipo. Isso é necessário para dar espaço a elementos de texto que serão inseridos programaticamente: o nome de exibição do editor, o título do plano, o resumo longo da oferta.
- A tela de fundo do logotipo Hero não pode estar em preto, branco ou transparente. Verifique se a cor do plano de fundo não é muito clara, pois o texto inserido será exibido em branco.
- Depois de publicar sua oferta com um ícone Hero, você não poderá removê-lo (embora seja possível atualizá-lo com uma versão diferente, se desejar).

Na seção **Gerenciamento de Leads**, você poderá selecionar o sistema CRM no qual seus leads serão armazenados. Observe que, de acordo com as [políticas de certificação de Serviços Gerenciados](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services), é necessário um **Destino de Lead**.

Por fim, forneça a **URL da Política de Privacidade** e os **Termos de Uso** na seção **Jurídico**. Você também pode especificar aqui se deseja ou não usar o [Contrato Standard](../../marketplace/standard-contract.md) na oferta.

Lembre-se de salvar as alterações antes de passar para a seção **Suporte**.

### <a name="add-support-info"></a>Adicionar informações de suporte

Na seção **Suporte**, forneça o nome, o email e o número de telefone de um contato de engenharia e de um contato do suporte ao cliente. Você também precisará fornecer URLs de suporte. A Microsoft poderá usar essas informações quando precisar entrar em contato com você sobre problemas comerciais e de suporte.

Depois de adicionar essas informações, selecione **Salvar.**

## <a name="publish-your-offer"></a>Publicar sua oferta

Depois de concluir todas as seções, a próxima etapa é publicar a oferta no Azure Marketplace. Selecione o botão **Publicar** para iniciar o processo de ativação da oferta. Para saber mais sobre esse processo, confira [Publicar ofertas do Azure Marketplace e do AppSource](../../marketplace/cloud-partner-portal/manage-offers/cpp-publish-offer.md).

Você pode [publicar uma versão atualizada de sua oferta](../../marketplace/cloud-partner-portal/manage-offers/cpp-update-offer.md) a qualquer momento. Por exemplo, talvez você queira adicionar uma nova definição de função a uma oferta publicada anteriormente. Quando você fizer isso, os clientes que já tiverem adicionado a oferta verão um ícone na página [**Provedores de serviço**](view-manage-service-providers.md) no portal do Azure avisando que há uma atualização disponível. Cada cliente poderá [examinar as alterações](view-manage-service-providers.md#update-service-provider-offers) e decidir se querem atualizar para a nova versão. 

## <a name="the-customer-onboarding-process"></a>O processo de integração do cliente

Após o cliente adicionar a oferta, ele poderá [delegar uma ou mais assinaturas ou grupos de recursos específicos](view-manage-service-providers.md#delegate-resources), que serão integrados para gerenciamento de recursos delegados do Azure. Se um cliente tiver aceitado uma oferta, mas ainda não tiver delegado nenhum recurso, ele verá uma observação na parte superior da seção **Ofertas de provedores** da página [**Provedores de serviço**](view-manage-service-providers.md) no portal do Azure.

> [!IMPORTANT]
> A delegação deve ser feita por uma conta que não seja de convidado no locatário do cliente que tem a [função interna de proprietário](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) para a assinatura que está sendo integrada (ou que contém os grupos de recursos que estão sendo integrados). Para ver todos os usuários que podem delegar a assinatura, um usuário do locatário do cliente poderá selecionar a assinatura no portal do Azure, abrir o **IAM (Controle de acesso)** e [exibir todos os usuários com a função Proprietário](../../role-based-access-control/role-assignments-list-portal.md#list-owners-of-a-subscription).

Depois que o cliente delega uma assinatura (ou um ou mais grupos de recursos em uma assinatura), o provedor de recursos **Microsoft. managedservices** será registrado para essa assinatura e os usuários em seu locatário poderão acessar os recursos delegados de acordo com as autorizações em sua oferta.

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre o [Marketplace comercial](../../marketplace/partner-center-portal/commercial-marketplace-overview.md).
- Saiba mais sobre as [experiências de gerenciamento entre locatários](../concepts/cross-tenant-management-experience.md).
- [Exiba e gerencie clientes](view-manage-customers.md) acessando **Meus clientes** no portal do Azure.
