---
title: Registrar um aplicativo Web que entra em usuários-plataforma de identidade da Microsoft | Azure
description: Saiba como registrar um aplicativo Web que faz logon em usuários
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 7d2eb5356b1abc54508fd6bf8d35fd9fc39d02ec
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80881572"
---
# <a name="web-app-that-signs-in-users-app-registration"></a>Aplicativo Web que assina usuários: registro de aplicativo

Este artigo explica as especificações de registro do aplicativo para um aplicativo Web que conecta os usuários.

Para registrar seu aplicativo, você pode usar:

- Os [guias de início rápido do aplicativo Web](#register-an-app-by-using-the-quickstarts). Além de ser uma ótima primeira experiência com a criação de um aplicativo, os guias de início rápido no portal do Azure contêm um botão chamado **fazer essa alteração para mim**. Você pode usar esse botão para definir as propriedades necessárias, mesmo para um aplicativo existente. Você precisará adaptar os valores dessas propriedades ao seu próprio caso. Em particular, a URL da API Web para seu aplicativo provavelmente será diferente do padrão proposto, o que também afetará o URI de saída.
- O portal do Azure para [registrar seu aplicativo manualmente](#register-an-app-by-using-the-azure-portal).
- PowerShell e ferramentas de linha de comando.

## <a name="register-an-app-by-using-the-quickstarts"></a>Registrar um aplicativo usando os guias de início rápido

Você pode usar esses links para inicializar a criação de seu aplicativo Web:

- [ASP.NET Core](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)
- [ASP.NET](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs)

## <a name="register-an-app-by-using-the-azure-portal"></a>Registrar um aplicativo usando o portal do Azure

> [!NOTE]
> O portal a ser usado é diferente dependendo se o aplicativo é executado no Microsoft Azure nuvem pública ou em uma nuvem nacional ou soberanas. Para obter mais informações, consulte [nuvens nacionais](./authentication-national-cloud.md#app-registration-endpoints).


1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante, ou uma conta pessoal da Microsoft. Como alternativa, entre no portal do Azure de escolha para a nuvem nacional.
1. Se sua conta fornecer acesso a mais de um locatário, selecione sua conta no canto superior direito. Em seguida, defina a sessão do portal para o locatário do Azure Active Directory (Azure AD) desejado.
1. No painel esquerdo, selecione o serviço **Azure Active Directory** e, em seguida, selecione **registros de aplicativo** > **novo registro**.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

1. Quando a página **Registrar um aplicativo** for exibida, insira as informações de registro do aplicativo:
   1. Escolha os tipos de conta com suporte para seu aplicativo. (Consulte [tipos de conta com suporte](./v2-supported-account-types.md).)
   1. Na seção **Nome**, insira um nome de aplicativo relevante que será exibido aos usuários do aplicativo. Por exemplo, digite **AspNetCore-webapp**.
   1. Para **URI de redirecionamento**, adicione o tipo de aplicativo e o destino do URI que aceitará respostas de token retornadas após a autenticação bem-sucedida. Por exemplo, digite **https://localhost:44321**. Em seguida, selecione **Registrar**.
1. Selecione o menu **Autenticação** e, em seguida, adicione as seguintes informações:
   1. Para **URL de resposta**, **https://localhost:44321/signin-oidc** adicione do tipo **Web**.
   1. Na seção **Configurações avançadas** , defina a **URL** de logout **https://localhost:44321/signout-oidc**para.
   1. Em **Concessão implícita**, selecione **Tokens de ID**.
   1. Clique em **Salvar**.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

1. Quando a página **Registrar um aplicativo** for exibida, insira as informações de registro do aplicativo:
   1. Escolha os tipos de conta com suporte para seu aplicativo. (Consulte [tipos de conta com suporte](./v2-supported-account-types.md).)
   1. Na seção **Nome**, insira um nome de aplicativo relevante que será exibido aos usuários do aplicativo. Por exemplo, digite **MailApp-openidconnect-v2**.
   1. Na seção **URI de redirecionamento (opcional)** , selecione **Web** na caixa de combinação e insira o seguinte URI **https://localhost:44326/** de redirecionamento:.
1. Selecione **Registrar** para criar o aplicativo.
1. Selecione o menu **autenticação** .
1. Na seção de | **concessão implícita** **Configurações avançadas**, selecione **tokens de ID**. Este exemplo requer que o [fluxo de concessão implícita](v2-oauth2-implicit-grant-flow.md) seja habilitado para conectar o usuário.
1. Clique em **Salvar**.

# <a name="java"></a>[Java](#tab/java)

1. Quando a **página registrar um aplicativo** for exibida, insira um nome de exibição para o aplicativo. Por exemplo, digite **Java-webapp**.
1. Selecione **contas em qualquer diretório organizacional e contas pessoais da Microsoft (por exemplo, Skype, Xbox, Outlook.com)** e, em seguida, selecione **aplicativo Web/API** para o **tipo de aplicativo**.
1. Selecione **registrar** para registrar o aplicativo.
1. No menu à esquerda, selecione **autenticação**. Em **URIs de redirecionamento**, selecione **Web**.

1. Insira dois URIs de redirecionamento: um para a página de entrada e outro para a página do grafo. Para ambos, use o mesmo host e número da porta, seguido por **/msal4jsample/Secure/AAD** para a página de entrada e **msal4jsample/Graph/me** para a página de informações do usuário.

   Por padrão, o exemplo usa:

   - **http://localhost:8080/msal4jsample/secure/aad**
   - **http://localhost:8080/msal4jsample/graph/me**

  Em seguida, selecione **salvar**.

1. Selecione **certificados & segredos** no menu.
1. Na seção **segredos do cliente** , selecione **novo segredo do cliente**e, em seguida:

   1. Insira uma descrição de chave.
   1. Selecione a duração da chave **em 1 ano**.
   1. Selecione **Adicionar**.
   1. Quando o valor da chave for exibido, copie-o para mais tarde. Esse valor não será exibido novamente ou poderá ser recuperado por outros meios.

# <a name="python"></a>[Python](#tab/python)

1. Quando a página **Registrar um aplicativo** for exibida, insira as informações de registro do aplicativo:
   1. Na seção **Nome**, insira um nome de aplicativo relevante que será exibido aos usuários do aplicativo. Por exemplo, digite **Python-webapp**.
   1. Altere os **tipos de conta com suporte** para **contas em qualquer diretório organizacional e contas pessoais da Microsoft (por exemplo, Skype, Xbox, Outlook.com)**.
   1. Na seção **URI de redirecionamento (opcional)** , selecione **Web** na caixa de combinação e insira o seguinte URI **http://localhost:5000/getAToken**de redirecionamento:.
1. Selecione **Registrar** para criar o aplicativo.
1. Na página **Visão geral** do aplicativo, localize o valor de **ID do aplicativo (cliente)** e registre-o para uso posterior. Você precisará dele para definir o arquivo de configuração do Visual Studio para este projeto.
1. No menu à esquerda, selecione **certificados & segredos**.
1. Na seção **segredos do cliente** , selecione **novo segredo do cliente**e, em seguida:

   1. Insira uma descrição de chave.
   1. Selecione uma duração de chave igual a **Em 1 ano**.
   1. Selecione **Adicionar**.
   1. Quando o valor da chave for exibido, copie-o. Você precisará dela mais tarde.
---

## <a name="register-an-app-by-using-powershell"></a>Registrar um aplicativo usando o PowerShell

> [!NOTE]
> Atualmente, o PowerShell do Azure AD cria aplicativos com apenas os seguintes tipos de conta com suporte:
>
> - MyOrg (contas neste diretório organizacional somente)
> - AnyOrg (contas em qualquer diretório organizacional)
>
> Você pode criar um aplicativo que conecte usuários com suas contas pessoais da Microsoft (por exemplo, Skype, Xbox ou Outlook.com). Primeiro, crie um aplicativo multilocatário. Os tipos de conta com suporte são contas em qualquer diretório organizacional. Em seguida, altere `signInAudience` a propriedade no manifesto do aplicativo do portal do Azure. Para obter mais informações, consulte a [etapa 1,3](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-3-AnyOrgOrPersonal#step-1-register-the-sample-with-your-azure-ad-tenant) no tutorial de ASP.NET Core. Você pode generalizar essa etapa para aplicativos Web em qualquer idioma.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Configuração de código do aplicativo](scenario-web-app-sign-user-app-configuration.md)
