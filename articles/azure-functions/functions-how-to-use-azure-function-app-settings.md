---
title: Definir configurações do aplicativo de funções no Azure
description: Saiba como definir configurações de aplicativos do Azure Functions.
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.topic: conceptual
ms.date: 08/14/2019
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 662a04dbcc39f3fa95b0098eb8fe556b18b3495b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79276940"
---
# <a name="manage-your-function-app"></a>Gerenciar seu aplicativo de funções 

No Azure Functions, um aplicativo de funções fornece o contexto de execução para suas funções individuais. Os comportamentos do aplicativo de funções se aplicam a todas as funções hospedadas por um aplicativo função de determinada. Todas as funções em um aplicativo de funções devem ser do mesmo [idioma](supported-languages.md). 

As funções individuais em um aplicativo de funções são implantadas juntas e dimensionadas juntas. Todas as funções no mesmo aplicativo de funções compartilham recursos, por instância, à medida que o aplicativo de funções é dimensionado. 

Cadeias de conexão, variáveis de ambiente e outras configurações de aplicativo são definidas separadamente para cada aplicativo de funções. Todos os dados que devem ser compartilhados entre os aplicativos de funções devem ser armazenados externamente em um repositório persistente.

Este artigo descreve como configurar e gerenciar seus aplicativos de funções. 

> [!TIP]  
> Muitas opções de configuração também podem ser gerenciadas usando o [CLI do Azure]. 

## <a name="get-started-in-the-azure-portal"></a>Introdução ao portal do Azure

Acesse o [portal do Azure] e entre usando sua conta do Azure. Na barra de pesquisa na parte superior do portal, digite o nome de seu aplicativo de funções e selecione-o na lista. Depois de selecionar o aplicativo de funções, você deverá ver esta página:

![Visão geral do aplicativo de funções no portal do Azure](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

Você pode navegar até tudo o que precisa para gerenciar seu aplicativo de funções na página Visão geral, em particular as **[configurações do aplicativo](#settings)** e os **[recursos da plataforma](#platform-features)**.

## <a name="application-settings"></a><a name="settings"></a>Configurações do aplicativo

A guia **configurações do aplicativo** mantém as configurações que são usadas pelo seu aplicativo de funções. Essas configurações são armazenadas criptografadas e você deve selecionar **Mostrar valores** para ver os valores no Portal. Você também pode acessar as configurações do aplicativo usando o CLI do Azure.

### <a name="portal"></a>Portal

Para adicionar uma configuração no portal, selecione **nova configuração de aplicativo** e adicione o novo par chave-valor.

![Configurações do aplicativo de funções no portal do Azure.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

### <a name="azure-cli"></a>CLI do Azure

O [`az functionapp config appsettings list`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-list) comando retorna as configurações de aplicativo existentes, como no exemplo a seguir:

```azurecli-interactive
az functionapp config appsettings list --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME>
```

O [`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) comando adiciona ou atualiza uma configuração de aplicativo. O exemplo a seguir cria uma configuração com uma chave `CUSTOM_FUNCTION_APP_SETTING` chamada e um valor `12345`de:


```azurecli-interactive
az functionapp config appsettings set --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--settings CUSTOM_FUNCTION_APP_SETTING=12345
```

### <a name="use-application-settings"></a>Usar configurações do aplicativo

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

Ao desenvolver um aplicativo de funções localmente, você deve manter cópias locais desses valores no arquivo de projeto local. Settings. JSON. Para obter mais informações, consulte [Local Settings File](functions-run-local.md#local-settings-file).

## <a name="platform-features"></a>Recursos da plataforma

![Guia Recursos da plataforma do aplicativo de funções.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

Aplicativos de funções executados e mantidos pela plataforma de Serviço de Aplicativo do Azure. Dessa forma, seus aplicativos de funções têm acesso à maioria dos recursos da principal plataforma de hospedagem na Web do Azure. Na guia **Recursos da plataforma** você acessa vários recursos da plataforma de Serviço de Aplicativo para uso em seus aplicativos de funções. 

> [!NOTE]
> Nem todos os recursos do Serviço de Aplicativo estão disponíveis quando um aplicativo de funções é executado no plano de hospedagem de Consumo.

O restante deste artigo se concentra nos seguintes recursos do serviço de aplicativo no portal do Azure que são úteis para as funções do:

+ [Editor do Serviço de Aplicativo](#editor)
+ [Console](#console)
+ [Ferramentas avançadas (Kudu)](#kudu)
+ [Opções de implantação](#deployment)
+ [CORS](#cors)
+ [Autenticação](#auth)

Para saber mais sobre como trabalhar com configurações da Serviço de Aplicativo, confira [Definir configurações do Serviço de Aplicativo do Azure](../app-service/configure-common.md).

### <a name="app-service-editor"></a><a name="editor"></a>Editor do Serviço de Aplicativo

![O Editor de Serviço de Aplicativo](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

O Editor do Serviço de Aplicativo é um editor avançado dentro do portal que você pode usar para modificar arquivos de configuração JSON e arquivos de código. Escolher essa opção inicia uma nova guia do navegador com um editor básico. Isso permite que você se integre ao repositório do Git, execute e depure código, além de modificar as configurações do aplicativos de funções. Esse editor fornece um ambiente de desenvolvimento aprimorado para suas funções comparadas com o editor de funções interno.  

Recomendamos que você considere o desenvolvimento de suas funções no computador local. Quando você desenvolve localmente e publica no Azure, seus arquivos de projeto são somente leitura no Portal. Para saber mais, confira [código e teste Azure Functions localmente](functions-develop-local.md).

### <a name="console"></a><a name="console"></a>Console

![Console do aplicativo de funções](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

O console no portal é uma ferramenta ideal de desenvolvimento quando você preferir interagir com seu aplicativo de funções na linha de comando. Os comandos comuns incluem criação e navegação de diretório e arquivo, bem como execução scripts e arquivos em lote. 

Ao desenvolver localmente, é recomendável usar o [Azure Functions Core Tools](functions-run-local.md) e o [CLI do Azure].

### <a name="advanced-tools-kudu"></a><a name="kudu"></a>Ferramentas avançadas (Kudu)

![Configurar Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)

As ferramentas avançadas para o Serviço de Aplicativo (também conhecidas como Kudu) fornecem acesso a recursos administrativos avançados de seu aplicativo de funções. No Kudu, você pode gerenciar informações do sistema, configurações do aplicativo, variáveis de ambiente, extensões do site, cabeçalhos HTTP e variáveis de servidor. Você também pode iniciar o **Kudu** navegando até o ponto de extremidade SCM de seu aplicativo de funções, como`https://<myfunctionapp>.scm.azurewebsites.net/` 


### <a name="deployment-center"></a><a name="deployment"></a>Centro de Implantação

Quando você usa uma solução de controle do código-fonte para desenvolver e manter seu código de funções, a central de implantação permite criar e implantar a partir do controle do código-fonte. Seu projeto é compilado e implantado no Azure quando você faz atualizações. Para obter mais informações, consulte [tecnologias de implantação no Azure Functions](functions-deployment-technologies.md).

### <a name="cross-origin-resource-sharing"></a><a name="cors"></a>Compartilhamento de recursos entre origens

Para evitar a execução de código mal-intencionado no cliente, os navegadores modernos bloqueiam solicitações de aplicativos Web para recursos em execução em um domínio separado. O [CORS (compartilhamento de recursos entre origens)](https://developer.mozilla.org/docs/Web/HTTP/CORS) permite `Access-Control-Allow-Origin` que um cabeçalho declare quais origens têm permissão para chamar pontos de extremidade em seu aplicativo de funções.

#### <a name="portal"></a>Portal

Quando você configura a lista de **origens permitidas** para seu aplicativo de `Access-Control-Allow-Origin` funções, o cabeçalho é automaticamente adicionado a todas as respostas de pontos de extremidade http em seu aplicativo de funções. 

![Configurar lista CORS do aplicativo de funções](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

Quando o curinga (`*`) é usado, todos os outros domínios são ignorados. 

Use o [`az functionapp cors add`](/cli/azure/functionapp/cors#az-functionapp-cors-add) comando para adicionar um domínio à lista de origens permitidas. O exemplo a seguir adiciona o domínio contoso.com:

```azurecli-interactive
az functionapp cors add --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--allowed-origins https://contoso.com
```

Use o [`az functionapp cors show`](/cli/azure/functionapp/cors#az-functionapp-cors-show) comando para listar as origens permitidas atualmente.

### <a name="authentication"></a><a name="auth"></a>Autenticação

![Configurar a autenticação para um aplicativo de funções](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)

Quando as funções usam um gatilho HTTP, você pode exigir que as chamadas sejam autenticadas primeiro. O serviço de aplicativo dá suporte à autenticação Azure Active Directory e a entrar com provedores sociais, como Facebook, Microsoft e Twitter. Para obter detalhes sobre como configurar provedores de autenticação específicos, consulte [Visão geral de autenticação do serviço de aplicativo do Azure](../app-service/overview-authentication-authorization.md). 


## <a name="next-steps"></a>Próximas etapas

+ [Definir configurações do Serviço de Aplicativo do Azure](../app-service/configure-common.md)
+ [Implantação contínua para Azure Functions](functions-continuous-deployment.md)

[CLI do Azure]: /cli/azure/
[Azure portal]: https://portal.azure.com
