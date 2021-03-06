---
title: Protocolos OAuth 2,0 e OpenID Connect-plataforma Microsoft Identity | Azure
description: Um guia para os protocolos OAuth 2,0 e OpenID Connect que são suportados pelo ponto de extremidade da plataforma Microsoft Identity.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 04/13/2020
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: e94bdd6e79ff13d8ba736e140538bae74091f727
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81680084"
---
# <a name="oauth-20-and-openid-connect-protocols-on-the-microsoft-identity-platform"></a>Protocolos OAuth 2,0 e OpenID Connect na plataforma de identidade da Microsoft

O ponto de extremidade da plataforma de identidade da Microsoft para identidade como serviço com protocolos padrão do setor, OpenID Connect e OAuth 2,0. Embora o serviço esteja em conformidade com o padrão, pode haver diferenças sutis entre duas implementações diferentes desses protocolos. As informações descritas aqui serão úteis se você optar por criar seu código enviando e tratando solicitações HTTP diretamente, ou usar uma biblioteca de software livre de terceiros ao invés de usar uma de nossas [bibliotecas de software livre](reference-v2-libraries.md).

## <a name="the-basics"></a>Noções básicas

Em quase todos os fluxos do OAuth 2.0 e do OpenID Connect, há quatro partes envolvidas na troca:

![Diagrama mostrando as funções do OAuth 2,0](./media/active-directory-v2-flows/protocols-roles.svg)

* O **servidor de autorização** é o ponto de extremidade da plataforma Microsoft Identity e é responsável por garantir a identidade do usuário, conceder e revogar o acesso a recursos e emitir tokens. O servidor de autorização também é conhecido como provedor de identidade; ele trata com segurança tudo que estiver relacionado às informações do usuário, seu acesso e as relações de confiança entre as partes de um fluxo.
* O **Proprietário do Recurso** normalmente é o usuário final. Ele é a parte que possui os dados e tem o poder de permitir que terceiros acessem esses dados ou recurso.
* O **Cliente OAuth** é o seu aplicativo, identificado pela respectiva ID de Aplicativo. Geralmente é a parte com a qual usuário final interage e solicita tokens do servidor de autorização. O cliente deve receber permissão do proprietário do recurso para acessar o recurso.
* O **Servidor de Recurso** é o local no qual o recurso ou os dados residem. Ele confia no servidor de autorização para autenticar e autorizar com segurança o cliente OAuth e usa tokens de acesso de portador para garantir que o acesso a um recurso possa ser concedido.

## <a name="app-registration"></a>Registro do aplicativo

Cada aplicativo que deseja aceitar contas pessoais e corporativas ou de estudante deve ser registrado por meio da experiência de **registros de aplicativo** no [portal do Azure](https://aka.ms/appregistrations) antes de poder assinar esses usuários usando o OAuth 2,0 ou o OpenID Connect. O processo de registro de aplicativo coleta e atribui alguns valores a seu aplicativo:

* Uma **ID de aplicativo** que identifica exclusivamente seu aplicativo
* Um **URI de redirecionamento** (opcional) que pode ser usado para direcionar as respostas de volta para seu aplicativo
* Alguns outros valores específicos de cenário.

Para obter mais detalhes, saiba como [registrar um aplicativo](quickstart-register-app.md).

## <a name="endpoints"></a>Pontos de extremidade

Depois de registrado, o aplicativo se comunica com a plataforma de identidade da Microsoft enviando solicitações ao ponto de extremidade:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Em que o `{tenant}` pode ter um de quatro valores diferente:

| Valor | Descrição |
| --- | --- |
| `common` | Permite que os usuários com contas pessoais da Microsoft e contas corporativas/de estudante do Azure AD entrem no aplicativo. |
| `organizations` | Permite que somente usuários com contas corporativas/de estudante do Azure AD entrem no aplicativo. |
| `consumers` | Permite que somente os usuários com MSA (Contas Pessoais da Microsoft) para entrem no aplicativo. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` ou `contoso.onmicrosoft.com` | Permite que somente os usuários com contas corporativas/de estudante de um locatário específico do Azure AD entrem no aplicativo. É possível usar o nome de domínio amigável do locatário do Azure AD ou o identificador GUID de locatário. |

Para saber mais sobre como interagir com esses pontos de extremidade, escolha um tipo específico de aplicativo na seção [Protocolos](#protocols) e siga os links.

> [!TIP]
> Qualquer aplicativo registrado no Azure AD pode usar o ponto de extremidade da plataforma de identidade da Microsoft, mesmo se eles não entrarem em contas pessoais.  Dessa forma, você pode migrar aplicativos existentes para a plataforma de identidade da Microsoft e [MSAL](reference-v2-libraries.md) sem recriar seu aplicativo.

## <a name="tokens"></a>Tokens

A implementação da plataforma Microsoft Identity do OAuth 2,0 e do OpenID Connect fazem uso extensivo de tokens de portador, incluindo tokens de portador representados como JWTs. Um token de portador é um token de segurança leve que concede ao "portador" acesso a um recurso protegido. Nesse sentido, o "portador" é qualquer parte que possa apresentar o token. Embora uma parte deva primeiro autenticar com a plataforma de identidade da Microsoft para receber o token de portador, se as etapas necessárias não forem tomadas para proteger o token na transmissão e no armazenamento, elas poderão ser interceptadas e usadas por uma parte não intencional. Embora alguns tokens de segurança tenham um mecanismo interno para impedir que partes não autorizadas os utilizem, tokens de portador não possuem esse mecanismo e devem ser transportados em um canal seguro, como segurança da camada de transporte (HTTPS). Se um token de portador for transmitido livremente, uma parte mal-intencionada pode usar um ataque "man-in-the-middle" para adquirir o token e usá-lo para um acesso não autorizado a um recurso protegido. Os mesmos princípios de segurança se aplicam ao armazenar ou manter em cache tokens de portador para uso posterior. Sempre se certifique de que seu aplicativo transmita e armazene tokens de portador de maneira segura. Para obter mais considerações de segurança sobre tokens de portador, consulte [RFC 6750 seção 5](https://tools.ietf.org/html/rfc6750).

Mais detalhes de diferentes tipos de tokens usados no ponto de extremidade da plataforma Microsoft Identity estão disponíveis na [referência de token do ponto de extremidade da plataforma de identidade da Microsoft](v2-id-and-access-tokens.md).

## <a name="protocols"></a>Protocolos

Se você estiver pronto para ver alguns exemplos de solicitação, inicie com um dos tutoriais a seguir. Cada um corresponde a um cenário de autenticação específico. Se precisar de ajuda para determinar qual é o fluxo certo para você, confira [os tipos de aplicativos que você pode criar com a plataforma de identidade da Microsoft](v2-app-types.md).

* [Crie aplicativos móveis e nativos com o OAuth 2,0](v2-oauth2-auth-code-flow.md)
* [Criar aplicativos Web com o OpenID Connect](v2-protocols-oidc.md)
* [Criar aplicativos de página única com o fluxo implícito do OAuth 2.0](v2-oauth2-implicit-grant-flow.md)
* [Criar daemons ou processos do servidor com o fluxo de credenciais do cliente OAuth 2.0](v2-oauth2-client-creds-grant-flow.md)
* [Obter tokens em uma API Web com o OAuth 2.0 em nome do fluxo](v2-oauth2-on-behalf-of-flow.md)
