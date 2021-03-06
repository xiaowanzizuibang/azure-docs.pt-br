---
title: Examinar usuário declarações-LUIS
titleSuffix: Azure Cognitive Services
description: Examine o declarações capturado pelo aprendizado ativo para selecionar entidades de intenção e marca para declarações do mundo de leitura; aceitar alterações, treinar e publicar.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/27/2020
ms.author: diberry
ms.openlocfilehash: 95b7c7446a47fafd26d00b0da4d880786340fcd0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79219847"
---
# <a name="how-to-improve-the-luis-app-by-reviewing-endpoint-utterances"></a>Como melhorar o aplicativo LUIS examinando o ponto de extremidade declarações

O processo de revisão do ponto de extremidade declarações para previsões corretas é chamado de [aprendizado ativo](luis-concept-review-endpoint-utterances.md). O aprendizado ativo captura consultas de ponto de extremidade e seleciona o ponto de extremidade do usuário declarações que não tem certeza de. Revise essas declarações para selecionar as entidades de intenção e marca para essas declarações de leitura do mundo. Aceite essas alterações em seu exemplo declarações, em seguida, treine e publique. LUIS, em seguida, identifica declarações com mais precisão.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="enable-active-learning"></a>Habilitar o aprendizado ativo

Para habilitar o aprendizado ativo, você deve registrar as consultas de usuário. Isso é feito chamando a [consulta de ponto](luis-get-started-create-app.md#query-the-v3-api-prediction-endpoint) de extremidade `log=true` com o parâmetro e o valor de QueryString.

Use o portal do LUIS para construir a consulta de ponto de extremidade correta.

1. No [portal do Luis de visualização](https://preview.luis.ai/), selecione seu aplicativo na lista de aplicativos.
1. Vá para a seção **gerenciar** e, em seguida, selecione **recursos do Azure**.
1. Para o recurso de previsão atribuído, selecione **alterar parâmetros de consulta**.

    > [!div class="mx-imgBorder"]
    > ![Use o portal do LUIS para salvar logs, o que é necessário para o aprendizado ativo.](./media/luis-tutorial-review-endpoint-utterances/azure-portal-change-query-url-settings.png)

1. Alterne **salvar logs** e salve selecionando **concluído**.

    > [!div class="mx-imgBorder"]
    > ![Use o portal do LUIS para salvar logs, o que é necessário para o aprendizado ativo.](./media/luis-tutorial-review-endpoint-utterances/luis-portal-manage-azure-resource-save-logs.png)

     Essa ação altera a URL de exemplo adicionando o parâmetro querystring `log=true`. Copie e use a URL de consulta de exemplo alterada ao fazer consultas de previsão para o ponto de extremidade do runtime.

## <a name="correct-intent-predictions-to-align-utterances"></a>Corrigir previsões de intenção para alinhar declarações

Cada declaração tem uma intenção sugerida exibida na coluna **Intenção alinhada**.

> [!div class="mx-imgBorder"]
> [![Examinar o ponto de extremidade declarações que o LUIS não tem certeza de](./media/label-suggested-utterances/review-endpoint-utterances.png)](./media/label-suggested-utterances/review-endpoint-utterances.png#lightbox)

Se você concordar com essa intenção, selecione a marca de seleção. Se você não concordar com a sugestão, selecione a intenção correta na lista suspensa de intenções alinhadas, e marque a caixa de seleção à direita da intenção alinhada. Depois de selecionar na marca de seleção, o expressão é movido para a intenção e removido da lista **examinar ponto de extremidade declarações** .

> [!TIP]
> É importante ir até a página de detalhes da intenção para revisar e corrigir as previsões de entidade de todos os exemplos declarações da lista **examinar ponto de extremidade declarações** .

## <a name="delete-utterance"></a>Excluir a declaração

Cada declaração pode ser excluída da lista de revisão. Depois de excluída, ela não aparecerá na lista novamente. Isso será verdadeiro mesmo se o usuário inserir a mesma declaração do ponto de extremidade.

Se você não tiver certeza se deve excluir o expressão, mova-o para a intenção nenhum ou crie uma nova intenção, como `miscellaneous` e mova o expressão para essa intenção.

## <a name="disable-active-learning"></a>Desabilitar o aprendizado ativo

Para desabilitar o aprendizado ativo, não faça log de consultas de usuário. Isso é feito definindo a [consulta de ponto](luis-get-started-create-app.md#query-the-v2-api-prediction-endpoint) de extremidade `log=false` com o parâmetro e o valor de QueryString ou não usando o valor de QueryString porque o valor padrão é false.

## <a name="next-steps"></a>Próximas etapas

Para testar como o desempenho melhora depois que você rotula as declarações sugeridas, você poderá acessar o console de teste selecionando **Teste** no painel superior. Para obter instruções sobre como testar seu aplicativo usando o console de teste, consulte [Treinar e testar seu aplicativo](luis-interactive-test.md).
