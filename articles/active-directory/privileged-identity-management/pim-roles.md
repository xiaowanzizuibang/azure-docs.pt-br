---
title: Funções que você não pode gerenciar em Privileged Identity Management Azure Active Directory | Microsoft Docs
description: Descreve as funções que não podem ser gerenciadas no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 03/31/2020
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: b6eaa50f57dd8037ef0ad96b69284f565bd3558f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80607535"
---
# <a name="roles-you-cant-manage-in-privileged-identity-management"></a>Funções que você não pode gerenciar no Privileged Identity Management

O Azure Active Directory (Azure AD) Privileged Identity Management (PIM) permite que você gerencie todas as [funções do Azure ad](../users-groups-roles/directory-assign-admin-roles.md) e todas as [funções do Azure](../../role-based-access-control/built-in-roles.md). As funções do Azure também podem incluir suas funções personalizadas anexadas aos grupos de gerenciamento, assinaturas, grupos de recursos e recursos. No entanto, há algumas poucas funções que não podem ser gerenciadas. Este artigo descreve as funções que você não pode gerenciar no Privileged Identity Management.

## <a name="classic-subscription-administrator-roles"></a>Funções de administrador de assinatura Clássico

Você não pode gerenciar as seguintes funções de administrador de assinatura clássica no Privileged Identity Management:

- Administrador de conta
- Administrador de serviços
- Coadministrador

Para saber mais sobre as funções administrador de assinatura clássica, confira o artigo [Funções de administrador de assinatura clássica, funções RBAC do Azure e funções de administrador do Azure AD](../../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="what-about-office-365-admin-roles"></a>E as funções de administrador do Office 365?

Damos suporte a todas as funções do Office365 na experiência do portal de funções e administradores do Azure AD, como administrador do Exchange e administrador do SharePoint, mas não damos suporte a funções específicas no RBAC do Exchange ou no RBAC do SharePoint. Para saber mais sobre esses serviços do Office 365, confira [Funções de administrador do Office 365](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles).

> [!NOTE]
> Os usuários qualificados para a função de administrador do SharePoint, bem como quaisquer funções que tentam acessar o centro de conformidade e segurança da Microsoft podem enfrentar atrasos de até algumas horas após a ativação de sua função. Estamos trabalhando com essas equipes para corrigir os problemas.

## <a name="next-steps"></a>Próximas etapas

- [Atribuir funções do Azure AD no Privileged Identity Management](pim-how-to-add-role-to-user.md)
- [Atribuir funções de recurso do Azure no Privileged Identity Management](pim-resource-roles-assign-roles.md)
