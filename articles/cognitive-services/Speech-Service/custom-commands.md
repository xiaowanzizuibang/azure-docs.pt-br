---
title: Comandos personalizados (versão prévia) – serviço de fala
titleSuffix: Azure Cognitive Services
description: Uma visão geral dos recursos, funcionalidades e restrições para comandos personalizados (versão prévia), uma solução para a criação de aplicativos de voz.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: travisw
ms.openlocfilehash: 2e1b6ee0bd6c392804915fac6ff23278a00b6d33
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79367832"
---
# <a name="what-are-custom-commands-preview"></a>O que são comandos personalizados (versão prévia)?

Os aplicativos de voz, como os [assistentes de voz](voice-assistants.md) , ouvem os usuários e tomam uma ação em resposta, muitas vezes falando de volta. Eles usam a [fala para o texto](speech-to-text.md) para transcrever a fala do usuário e, em seguida, tomar medidas sobre a compreensão do idioma natural do texto. Essa ação geralmente inclui a saída falada do Assistente gerado com [conversão de texto em fala](text-to-speech.md). Os dispositivos se conectam aos assistentes com o `DialogServiceConnector` objeto do SDK de fala.

Os **comandos personalizados (versão prévia)** são uma solução simplificada para a criação de aplicativos de voz. Ele fornece uma experiência de criação unificada, um modelo de hospedagem automática e complexidade relativamente menor em relação a outras opções, como a [Direct line Speech](direct-line-speech.md). No entanto, essa simplificação vem com uma redução na flexibilidade. Assim, os comandos personalizados (versão prévia) são mais adequados para os cenários de conclusão de tarefas ou de comando e controle. Ele é particularmente bem correspondido para dispositivos de Internet das Coisas (IoT) e sem periféricos.

Para interação e integração de conversas complexas com outras soluções, como a [solução de assistente virtual e modelo empresarial](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview) , você é incentivado a usar a fala de linha direta.

Bons candidatos para comandos personalizados (versão prévia) têm um vocabulário fixo com conjuntos bem definidos de variáveis. Por exemplo, as tarefas de automação doméstica, como controlar um termostato, são ideais.

   ![Exemplos de cenários de conclusão de tarefas](media/voice-assistants/task-completion-examples.png "exemplos de conclusão de tarefas")

## <a name="getting-started-with-custom-commands-preview"></a>Introdução aos comandos personalizados (visualização)

A primeira etapa para usar comandos personalizados (versão prévia) para criar um aplicativo de voz é [obter uma chave de assinatura de fala](get-started.md) e acessar o construtor de comandos personalizados (versão prévia) no [Speech Studio](https://speech.microsoft.com). A partir daí, você pode criar um novo aplicativo de comandos personalizados (versão prévia) e publicá-lo, após o qual um aplicativo no dispositivo pode se comunicar com ele usando o SDK de fala.

   ![Fluxo de criação para comandos personalizados (versão prévia)](media/voice-assistants/custom-commands-flow.png "O fluxo de criação de comandos personalizados (visualização)")

Oferecemos guias de início rápido projetados para que você execute códigos em menos de 10 minutos.

* [Criar um aplicativo de comandos personalizados (versão prévia)](quickstart-custom-speech-commands-create-new.md)
* [Criar um aplicativo de comandos personalizados (versão prévia) com parâmetros](quickstart-custom-speech-commands-create-parameters.md)
* [Conectar-se a um aplicativo de comandos personalizados (versão prévia) com o SDK de fala, C #](quickstart-custom-speech-commands-speech-sdk.md)

Depois de concluir os guias de início rápido, Explore nosso "How-tos".

- [Adicionar validações a parâmetros de comando personalizados](./how-to-custom-speech-commands-validations.md)
- [Preencher comandos no cliente com o SDK de fala](./how-to-custom-speech-commands-fulfill-sdk.md)
- [Adicionar uma confirmação a um Comando Personalizado](./how-to-custom-speech-commands-confirmations.md)
- [Adicionar uma correção de uma etapa a um Comando Personalizado](./how-to-custom-speech-commands-one-step-correction.md)

## <a name="next-steps"></a>Próximas etapas

* [Obter gratuitamente uma chave de assinatura dos Serviços de Fala](get-started.md)
* [Vá para o Speech Studio para experimentar comandos personalizados](https://speech.microsoft.com)
* [Obter o SDK de Fala](speech-sdk.md)
