---
title: 'Como: adicionar uma confirmação a um comando personalizado (visualização)'
titleSuffix: Azure Cognitive Services
description: Neste artigo, como implementar confirmações para um comando em comandos personalizados.
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: afa197c83b4f66f12863de4185ef7763447f3ed9
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75456493"
---
# <a name="how-to-add-a-confirmation-to-a-custom-command-preview"></a>Como: adicionar uma confirmação a um comando personalizado (visualização)

Neste artigo, você aprenderá a adicionar uma confirmação a um comando.

## <a name="prerequisites"></a>Pré-requisitos

Você deve ter concluído as etapas nos seguintes artigos:

- [Início rápido: criar um comando personalizado (versão prévia)](./quickstart-custom-speech-commands-create-new.md)
- [Início rápido: criar um comando personalizado com parâmetros (versão prévia)](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-setalarm-command"></a>Criar um comando setAlarm

Para demonstrar validações, vamos criar um novo comando, permitindo que o usuário defina um alarme.

1. Abra o aplicativo de comandos personalizados criado anteriormente no [Speech Studio](https://speech.microsoft.com/)
1. Criar um novo comando **setAlarm**
1. Adicionar um parâmetro chamado DateTime

   | Configuração           | Valor sugerido                                          | Descrição                                                                                      |
   | ----------------- | ---------------------------------------------------------| ------------------------------------------------------------------------------------------------ |
   | Nome              | Datetime                                                 | Um nome descritivo para o parâmetro de comando                                                    |
   | Obrigatório          | true                                                     | Caixa de seleção que indica se um valor para esse parâmetro é necessário antes de concluir o comando |
   | Modelo de resposta | "-Qual é a hora?"                                           | Um prompt para solicitar o valor desse parâmetro quando ele não for conhecido                              |
   | Type              | Datetime                                                 | O tipo de parâmetro, como número, Cadeia de caracteres ou data e hora                                      |
   | Padrões de data     | Se a data estiver ausente, use hoje mesmo                             |                                                                                                  |
   | Padrões de tempo     | Se o tempo estiver ausente, use o início do dia                      |                                                                                                  | 

1. Adicionar algumas frases de exemplo
   
   ```
    set an alarm for {DateTime}
    set alarm {DateTime}
    alarm for {DateTime}
   ```

1. Adicionar uma regra de conclusão para confirmar o resultado

   | Configuração    | Valor sugerido                                         | Descrição                                        |
   | ---------- | ------------------------------------------------------- | -------------------------------------------------- |
   | Nome da Regra  | Definir alarme                                               | Um nome que descreve a finalidade da regra          |
   | Ações    | SpeechResponse-"-Ok, alarme definido para {DateTime}"       | A ação a ser tomada quando a condição da regra for verdadeira |

## <a name="try-it-out"></a>Experimente

Selecione o painel de teste e tente algumas interações.

- Entrada: Definir alarme para amanhã às 12h
- Saída: "Ok, alarme definido para 12/06/2019 12:00:00"

- Entrada: definir um alarme
- Saída: "qual vez?"
- Entrada: 17:00
- Saída: "Ok, alarme definido para 12/05/2019 17:00:00"

## <a name="add-the-advanced-rules-for-confirmation"></a>Adicionar as regras avançadas para confirmação

1. Adicione uma regra avançada para confirmação. 

    Essa regra solicitará que o usuário confirme a data e a hora do alarme e está esperando uma confirmação (Sim/não) para a próxima vez.

   | Configuração               | Valor sugerido                                                                  | Descrição                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Nome da Regra             | Confirmar data e hora                                                                | Um nome que descreve a finalidade da regra          |
   | Condições            | Parâmetro necessário-DateTime                                                    | Condições que determinam quando a regra pode ser executada    |   
   | Ações               | SpeechResponse-"-tem certeza de que deseja definir um alarme para {DateTime}?"       | A ação a ser tomada quando a condição da regra for verdadeira |
   | Estado após a execução | Esperar entrada                                                                   | Estado do usuário após a ativação                  |
   | As          | Confirmação                                                                     | Expectativa para a próxima rodada                      |

1. Adicione uma regra avançada para lidar com uma confirmação bem-sucedida (o usuário disse sim)

   | Configuração               | Valor sugerido                                                                  | Descrição                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Nome da Regra             | Confirmação aceita                                                            | Um nome que descreve a finalidade da regra          |
   | Condições            | SuccessfulConfirmation & parâmetro necessário-DateTime                           | Condições que determinam quando a regra pode ser executada    |   
   | Estado após a execução | Pronto para conclusão                                                             | Estado do usuário após a ativação                   |

1. Adicione uma regra avançada para lidar com uma confirmação negada (o usuário não disse)

   | Configuração               | Valor sugerido                                                                  | Descrição                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Nome da Regra             | Confirmação negada                                                                   | Um nome que descreve a finalidade da regra          |
   | Condições            | DeniedConfirmation & parâmetro necessário-DateTime                               | Condições que determinam quando a regra pode ser executada    |   
   | Ações               | ClearParameter-DateTime & SpeechResponse-"-sem problema, que tempo então?"     | A ação a ser tomada quando a condição da regra for verdadeira |
   | Estado após a execução | Esperar entrada                                                                   | Estado do usuário após a ativação                   |
   | As          | Extrairparameters-DateTime                                                      | Expectativa para a próxima rodada                      |

## <a name="try-it-out"></a>Experimente

Selecione o painel de teste e tente algumas interações.

- Entrada: Definir alarme para amanhã às 12h
- Saída: "tem certeza de que deseja definir um alarme para 12/07/2019 12:00:00?"
- Entrada: não
- Saída: "sem problema, que tempo então?"
- Entrada: 17:00
- Saída: "tem certeza de que deseja definir um alarme para 12/06/2019 17:00:00?"
- Entrada: Sim
- Saída: "Ok, alarme definido para 12/06/2019 17:00:00"

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Como: adicionar uma correção de uma etapa a um comando personalizado (versão prévia)](./how-to-custom-speech-commands-one-step-correction.md)