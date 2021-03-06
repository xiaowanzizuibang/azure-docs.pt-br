---
title: Migração de conta do Portal do Cloud Partner para o Partner Center-Marketplace comercial para o Azure
description: Como migrar sua conta do CPP para o Partner Center-Marketplace comercial para o Azure
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: 49fb2da05b7284fff10e6271de177551000995ed
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82024414"
---
# <a name="account-migration-from-cloud-partner-portal-to-partner-center"></a>Migração de conta do Portal do Cloud Partner para o Partner Center

Se você tiver uma conta de Portal do Cloud Partner (CPP) existente, suas configurações de conta precisarão ser migradas para o Partner Center.

## <a name="account-migration-process"></a>Processo de migração de conta

Se você for um usuário com a função de proprietário em CPP para uma determinada conta, uma faixa amarela será mostrada na página de perfil do Publicador. Você pode pertencer a um dos dois casos a seguir:

- Sua conta já foi migrada e você está pronto para gerenciar as configurações de conta no Partner Center.
- Sua conta não foi migrada para o Partner Center e você precisa realizar uma ação adicional.

### <a name="your-account-has-been-migrated-to-partner-center"></a>Sua conta foi migrada para o Partner Center

Para todas as contas que concluíram a migração do CPP para o Partner Center, o gerenciamento de conta ocorrerá no Partner Center. Alterações como adição/exclusão de usuário serão sincronizadas de volta para CPP.

### <a name="you-have-not-yet-migrated-your-account-to-partner-center"></a>Você ainda não migrou sua conta para o Partner Center

Clique na faixa para iniciar o processo de migração de sua conta. Você deve inserir os seguintes itens:

1. Endereço de email de trabalho: <br> <br> Na maioria dos casos, entre com o endereço de email que você usa para entrar em CPP. Em determinados casos, um endereço de email diferente deve ser usado:

    * Conta Microsoft: se a conta de CPP for uma conta Microsoft, insira um email de trabalho válido associado ao locatário para o qual a ID de Microsoft Partner Network (MPN) está registrada. Para obter mais informações, consulte [inscrever-se para Microsoft Partner Network programa](#sign-up-for-microsoft-partner-network-program).

    * Incompatibilidade de locatário: se seu endereço de email de trabalho não pertencer ao locatário associado à ID de Microsoft Partner Network presente na sua conta de CPP, você verá um erro. Para mover após esse erro, insira um endereço de email associado ao locatário. Uma mensagem de erro fornecerá o nome do locatário.

2. Inscrever-se no programa Microsoft Partner Network

    Se sua conta de CPP não tiver uma ID de Microsoft Partner Network ou tiver uma que seja inválida, você precisará se inscrever no programa de Microsoft Partner Network como parte do processo de ativação.

## <a name="publishers-moving-from-cpp"></a>Publicadores mudando de CPP

Se sua conta tiver sido migrada do [portal do Cloud Partner (CPP)](https://cloudpartner.azure.com), você não precisará criar uma nova conta do Partner Center. Você deve ter recebido um link personalizado para sua nova conta do Partner Center por email e em uma notificação de banner depois de fazer logon na sua conta de CPP existente.

Depois de habilitar sua nova conta do Partner Center visitando este link personalizado, você pode retornar à sua conta visitando o painel do [Marketplace comercial](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) no Partner Center.

As informações do contrato de publicação e do perfil da empresa serão migradas para sua nova conta do Partner Center, juntamente com as informações de perfil de pagamento da conta previamente configuradas, contas de usuário e permissões e ofertas ativas associadas à sua conta de CPP.

Depois que as informações da conta forem movidas do CPP para o Partner Center, você não usará mais o CPP para fazer atualizações de conta ou gerenciar usuários, permissões e cobrança. Por um período limitado, as atualizações de conta feitas no Partner Center serão automaticamente atualizadas em sua conta de CPP somente leitura até que o portal de CPP seja eventualmente preterido.

## <a name="sign-up-for-microsoft-partner-network-program"></a>Inscrever-se no programa Microsoft Partner Network

As empresas que desejam fazer parceria com a Microsoft devem ingressar no Microsoft Partner Network (MPN) e obter uma ID de MPN. Se você já é membro do Microsoft Partner Network e tem uma ID MPN, mantenha as informações à mão, pois você precisará dela durante o processo de ativação da conta.  

Se você não for membro da Microsoft Partner Network, poderá [ingressar aqui](https://signup.microsoft.com/signup?sku=StoreForBusinessIW&origin=partnerdashboard&culture=en-us&ru=https://partner.microsoft.com/dashboard/account/v3/xpu/onboard?ru=/dashboard/account/v3/enrollment/companyprofile/basicpartnernetwork/new) para obter uma ID de MPN. Anote sua ID do MPN, pois você precisará inseri-la durante o processo de ativação da conta.

Para saber mais sobre o Microsoft Partner Network, consulte [ingressar o Microsoft Partner Network](https://partner.microsoft.com/en-US/membership) no site do parceiro. Para saber mais sobre os benefícios do ISV na Microsoft Partner Network, consulte o [Hub de recursos do ISV](https://partner.microsoft.com/isv-resource-hub).  

## <a name="move-dynamics-365-and-powerapps-offers-to-partner-center"></a>Mover as ofertas do Dynamics 365 e do PowerApps para o Partner Center

Para simplificar a conta e oferecer gerenciamento para o engajamento do cliente do Dynamics 365, o PowerApps e as operações do Dynamics 365, as ofertas foram movidas para o [Partner Center](https://partner.microsoft.com/). A movimentação garante que o mesmo conteúdo esteja disponível para catálogos públicos e do vendedor.

Para obter informações específicas sobre o que precisa ser feito em **15 de outubro de 2019** para suas ofertas de operações do engajamento do cliente do Dynamics 365, do PowerApps e do Dynamics 365, siga as instruções abaixo.

> [!NOTE]
> Isso não se aplica às ofertas do Dynamics 365 Business central.  

1. Se sua conta de associação do MPN foi originalmente criada no centro de associação de parceiros (PMC), entre no [Partner Center](https://partner.microsoft.com/pcv/accountsettings/connectedpartnerprofile) para confirmar que sua conta foi migrada. Se você vir uma tela de perfil com sua ID do MPN, você estará pronto para continuar. Caso contrário, você precisará iniciar a migração da sua conta seguindo os prompts no [centro de associação do parceiro](https://partners.microsoft.com/partnerprogram/Welcome.aspx). Se precisar de ajuda, visite [suporte](https://partner.microsoft.com/support?issueid=100-0077).
2. Vá para a [página Visão geral do Marketplace comercial no Partner Center](https://partner.microsoft.com/dashboard/commercial-marketplace/overview). Se você vir "marketplace comercial" no painel de navegação esquerdo, você estará registrado e deverá continuar para a próxima etapa. Caso contrário, [Registre-se no Marketplace comercial](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) agora.
3. Confirme se suas ofertas estão em AppSource [pesquisando suas ofertas](https://appsource.microsoft.com/). Se suas ofertas já estiverem em AppSource, vá para a próxima etapa. Para qualquer oferta que não esteja em AppSource, crie uma [nova oferta de compromisso do cliente do dynamics 365](create-new-customer-engagement-offer.md) ou uma [nova oferta de operações do Dynamics 365](create-new-operations-offer.md).
4. Na [página contratos](https://partner.microsoft.com/dashboard/account/agreements)do Partner Center, verifique se você analisou e aceitou o **adendo Business Applications ISV**.
5. Nas [configurações de conta](https://partner.microsoft.com/dashboard/account/v3/accountsettings/billingprofile)do Partner Center, verifique se as informações de cobrança estão concluídas.
6. Envie cada oferta nova e existente para certificação e publicação, mesmo que suas ofertas tenham sido certificadas anteriormente.
    * Conclua as telas de informações, incluindo fornecer seu aplicativo para certificação, bem como informações de marketing. Selecione **Enviar** (canto superior direito da tela) em **15 de outubro de 2019**. Essas etapas devem ser concluídas para evitar o impacto da disponibilidade da oferta.
    * Se qualificado, você pode solicitar a participação na camada Premium durante esse processo.
    * A certificação ou a recertificação exige que seu aplicativo dê suporte à versão mais recente de nossa plataforma de Business Applications.
    * Depois que seu aplicativo tiver sido aprovado, você receberá um email para retornar à oferta e selecionar "entrar em tempo real" para permitir que a oferta fique ativa em Microsoft AppSource.

## <a name="additional-resources"></a>Recursos adicionais

Obtenha ajuda de especialistas e colegas em fóruns e descubra Blogs, webinars, vídeos, eventos e muito mais no [Microsoft Dynamics CRM](https://community.dynamics.com/crm?wa=wsignin1.0).

Se você precisar de ajuda para publicar, certificar ou gerenciar suas ofertas do Marketplace, [envie um tíquete de suporte](https://partner.microsoft.com/support/v2/?stage=1).

## <a name="next-step"></a>Próxima etapa

- [Gerenciar sua conta do Marketplace comercial no Partner Center](./manage-account.md)
