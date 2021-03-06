---
title: Como a depuração remota de seu código com Azure Dev Spaces funciona
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Descreve os processos de depuração remota no serviço kubernetes do Azure com Azure Dev Spaces
keywords: Azure Dev Spaces, espaços de desenvolvimento, Docker, kubernetes, Azure, AKS, serviço kubernetes do Azure, contêineres
ms.openlocfilehash: a9f303ea6f954285a319be137a594f7ce127f740
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80241394"
---
# <a name="how-remote-debugging-your-code-with-azure-dev-spaces-works"></a>Como a depuração remota de seu código com Azure Dev Spaces funciona

Azure Dev Spaces fornece várias maneiras de iterar e depurar rapidamente aplicativos kubernetes e colaborar com sua equipe em um cluster do AKS (serviço kubernetes do Azure). Depois que o projeto estiver sendo executado em um espaço de desenvolvimento, Azure Dev Spaces fornecerá uma maneira de anexar e depurar um aplicativo em execução no AKS.

Este artigo descreve como a depuração remota com espaços de desenvolvimento funciona.

## <a name="debug-your-code"></a>Depurar seu código

Para aplicativos Java, .NET Core e node. js, você pode depurar seu aplicativo em execução diretamente em seu espaço de desenvolvimento usando o Visual Studio Code ou o Visual Studio. Visual Studio Code e o Visual Studio fornecem ferramentas para se conectar ao seu espaço de desenvolvimento, iniciar o aplicativo e anexar um depurador. Após a `azds prep`execução, você pode abrir seu projeto no Visual Studio Code ou no Visual Studio. Visual Studio Code ou o Visual Studio gerará seus próprios arquivos de configuração para conexão, que é `azds prep`separado da execução. De dentro do Visual Studio Code ou do Visual Studio, você pode definir pontos de interrupção e iniciar seu aplicativo para o espaço de desenvolvimento.

![Depurando seu código](media/get-started-node/debug-configuration-nodejs2.png)

Quando você inicia seu aplicativo usando Visual Studio Code ou o Visual Studio para depuração, eles lidam com a inicialização e a conexão ao seu espaço de desenvolvimento `azds up`da mesma maneira que a execução. Além disso, as ferramentas do lado do cliente no Visual Studio Code e no Visual Studio fornecem um parâmetro adicional com informações específicas para depuração. O parâmetro contém o nome da imagem do depurador, o local do depurador dentro da imagem do depurador e o local de destino dentro do contêiner do aplicativo para montar a pasta do depurador.

A imagem do depurador é determinada automaticamente pelas ferramentas do lado do cliente. Ele usa um método semelhante ao usado durante a geração do gráfico Dockerfile e Helm durante a `azds prep`execução. Depois que o depurador é montado na imagem do aplicativo, ele é executado usando `azds exec`.

## <a name="next-steps"></a>Próximas etapas

Para começar a usar o Azure Dev Spaces para a depuração remota de seu projeto, consulte os seguintes guias de início rápido:

* [Iterar e depurar rapidamente com Visual Studio Code e Java][quickstart-java]
* [Iterar e depurar rapidamente com o Visual Studio Code e o .NET][quickstart-netcore]
* [Iterar e depurar rapidamente com Visual Studio Code e node. js][quickstart-node]
* [Iterar e depurar rapidamente com o Visual Studio e o .NET Core][quickstart-vs]


[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md
[quickstart-vs]: quickstart-netcore-visualstudio.md
