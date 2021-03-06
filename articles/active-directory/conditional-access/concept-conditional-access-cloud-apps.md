---
title: Aplicativos de nuvem ou ações na política de acesso condicional-Azure Active Directory
description: O que são aplicativos de nuvem ou ações em uma política de acesso condicional do Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: b15b022726c09ccbaf9674775d114c8dd1916e1d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81457290"
---
# <a name="conditional-access-cloud-apps-or-actions"></a>Acesso condicional: aplicativos de nuvem ou ações

Os aplicativos de nuvem ou as ações são um sinal de chave em uma política de acesso condicional. As políticas de acesso condicional permitem que os administradores atribuam controles a aplicativos ou ações específicas.

- Os administradores podem escolher entre a lista de aplicativos que incluem aplicativos internos da Microsoft e todos os [aplicativos integrados do Azure ad](../manage-apps/what-is-application-management.md) , incluindo Galeria, não galeria e aplicativos publicados por meio do [proxy de aplicativo](../manage-apps/what-is-application-proxy.md).
- Os administradores podem optar por definir a política não com base em um aplicativo de nuvem, mas em uma ação do usuário. A única ação com suporte é registrar informações de segurança (versão prévia), permitindo o acesso condicional para impor controles em relação à [experiência de registro de informações de segurança combinadas](../authentication/howto-registration-mfa-sspr-combined.md).

![Definir uma política de acesso condicional e especificar aplicativos de nuvem](./media/concept-conditional-access-cloud-apps/conditional-access-cloud-apps-or-actions.png)

## <a name="microsoft-cloud-applications"></a>Aplicativos em nuvem da Microsoft

Muitos dos aplicativos de nuvem da Microsoft existentes estão incluídos na lista de aplicativos que você pode selecionar. 

Os administradores podem atribuir uma política de acesso condicional aos seguintes aplicativos de nuvem da Microsoft. Alguns aplicativos como o Office 365 (visualização) e o gerenciamento de Microsoft Azure incluem vários aplicativos ou serviços filho relacionados. A lista a seguir não é exaustiva e está sujeita a alterações.

- [Office 365 (versão prévia)](#office-365-preview)
- Azure Analysis Services
- Azure DevOps
- [Banco de Dados SQL do Azure e SQL Data Warehouse do Azure](../../sql-database/sql-database-conditional-access.md)
- Dynamics CRM Online
- Análise do Microsoft Application Insights
- [Proteção de Informações do Microsoft Azure](/azure/information-protection/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)
- [Gerenciamento do Microsoft Azure](#microsoft-azure-management)
- Gerenciamento de assinatura Microsoft Azure
- Microsoft Cloud App Security
- Portal de controle de acesso de ferramentas do Microsoft Commerce
- Serviço de autenticação de ferramentas do Microsoft Commerce
- Microsoft Flow
- Microsoft Forms
- Microsoft Intune
- [Registro do Microsoft Intune](/intune/enrollment/multi-factor-authentication)
- Microsoft Planner
- Microsoft PowerApps
- Pesquisa da Microsoft no Bing
- Microsoft StaffHub
- Microsoft Stream
- Equipes da Microsoft
- Office 365 Exchange Online
- Office 365 SharePoint Online
- Office 365 Yammer
- Office Delve
- Sway do Office
- Outlook Groups
- Serviço do Power BI
- Project Online
- Skype for Business Online
- VPN (rede virtual privada)
- Windows Defender ATP

### <a name="office-365-preview"></a>Office 365 (versão prévia)

O Office 365 fornece serviços de produtividade e colaboração baseados em nuvem, como o Exchange, o SharePoint e o Microsoft Teams. Os serviços de nuvem do Office 365 estão profundamente integrados para garantir experiências suaves e colaborativas. Essa integração pode causar confusão ao criar políticas, já que alguns aplicativos, como o Microsoft Teams, têm dependências em outras pessoas, como o SharePoint ou o Exchange.

O aplicativo Office 365 (versão prévia) torna possível direcionar esses serviços de uma só vez. É recomendável usar o novo aplicativo Office 365 (versão prévia), em vez de direcionar aplicativos de nuvem individuais. Direcionar esse grupo de aplicativos ajuda a evitar problemas que podem surgir devido a políticas e dependências inconsistentes.

Os administradores podem optar por excluir aplicativos específicos da política se quiserem incluindo o aplicativo Office 365 (versão prévia) e excluindo os aplicativos específicos de sua escolha na política.

Aplicativos-chave incluídos no aplicativo cliente do Office 365 (versão prévia):

   - Microsoft Flow
   - Microsoft Forms
   - Microsoft Stream
   - Microsoft To-Do
   - Equipes da Microsoft
   - Office 365 Exchange Online
   - Office 365 SharePoint Online
   - Serviço de Pesquisa do Office 365
   - Office 365 Yammer
   - Office Delve
   - Office Online
   - Office.com
   - OneDrive
   - PowerApps
   - Skype for Business Online
   - Sway

### <a name="microsoft-azure-management"></a>Gerenciamento do Microsoft Azure

O aplicativo de gerenciamento de Microsoft Azure inclui vários serviços subjacentes. 

   - Portal do Azure
   - Provedor de Azure Resource Manager
   - APIs do modelo de implantação clássica
   - Azure PowerShell
   - Portal do administrador de assinaturas do Visual Studio
   - Azure DevOps
   - Portal de Azure Data Factory

> [!NOTE]
> O aplicativo de gerenciamento de Microsoft Azure aplica-se a Azure PowerShell, que chama a API Azure Resource Manager. Não se aplica ao PowerShell do Azure Active Directory, que chama o Microsoft Graph.

## <a name="other-applications"></a>Outros aplicativos

Além dos aplicativos da Microsoft, os administradores podem adicionar qualquer aplicativo registrado do Azure AD às políticas de acesso condicional. Esses aplicativos podem incluir: 

- Aplicativos publicados por meio [do Azure proxy de aplicativo do AD](../manage-apps/what-is-application-proxy.md)
- [Aplicativos adicionados da Galeria](../manage-apps/add-application-portal.md)
- [Aplicativos personalizados que não estão na Galeria](../manage-apps/add-non-gallery-app.md)
- [Aplicativos herdados publicados por meio de redes e controladores de entrega de aplicativos](../manage-apps/secure-hybrid-access.md)

## <a name="user-actions"></a>Ações do usuário

As ações do usuário são tarefas que podem ser executadas por um usuário. A única ação atualmente suportada é **registrar informações de segurança**, o que permite que a política de acesso condicional seja aplicada quando os usuários que estão habilitados para o registro combinado tentarem registrar suas informações de segurança. Mais informações podem ser encontradas no artigo registro de [informações de segurança combinadas](../authentication/concept-registration-mfa-sspr-combined.md).

## <a name="next-steps"></a>Próximas etapas

- [Acesso condicional: condições](concept-conditional-access-conditions.md)

- [Políticas comuns de acesso condicional](concept-conditional-access-policy-common.md)
- [Dependências do aplicativo cliente](service-dependencies.md)
