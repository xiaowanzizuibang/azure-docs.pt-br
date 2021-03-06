---
title: Segurança e autenticação da Grade de Eventos do Azure
description: Descreve a Grade de Eventos do Azure e seus conceitos.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: babanisa
ms.openlocfilehash: 03bc2f9de6f50f08c9f62f86a3d1791a067cecd0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78899276"
---
# <a name="authorizing-access-to-event-grid-resources"></a>Autorizando o acesso aos recursos da grade de eventos
A Grade de Eventos do Azure permite que você controle o nível de acesso concedido a usuários diferentes para execução de várias operações de gerenciamento, como listar assinaturas de evento, criar novos e gerar chaves. O Event Grid usa o controle de acesso baseado em função (RBAC) do Azure.

## <a name="operation-types"></a>Tipos de operação

Grade de eventos suporta as seguintes ações:

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

As últimas três operações retornam informações possivelmente secretas, as quais são filtradas dentre operações de leitura normais. É recomendável que você restrinja o acesso a essas operações. 

## <a name="built-in-roles"></a>Funções internas

Grade de eventos fornece duas funções internas para gerenciar assinaturas de evento. Eles são importantes ao implementar [domínios de evento](event-domains.md) porque fornecem aos usuários as permissões de que eles precisam para assinar tópicos em seu domínio de evento. Essas funções são focadas em assinaturas de eventos e não concedem acesso a ações como a criação de tópicos.

Você pode [atribua essas funções para um usuário ou grupo](../role-based-access-control/quickstart-assign-role-user-portal.md).

**Colaborador do EventGrid EventSubscription**: gerenciar operações de assinatura da grade de eventos

```json
[
  {
    "Description": "Lets you manage EventGrid event subscription operations.",
    "IsBuiltIn": true,
    "Id": "428e0ff05e574d9ca2212c70d0e0a443",
    "Name": "EventGrid EventSubscription Contributor",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/*",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Resources/deployments/*",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "Condition": null
      }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

**Leitor de EventSubscription EventGrid**: ler assinaturas de grade de eventos

```json
[
  {
    "Description": "Lets you read EventGrid event subscriptions.",
    "IsBuiltIn": true,
    "Id": "2414bbcf64974faf8c65045460748405",
    "Name": "EventGrid EventSubscription Reader",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/read",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": []
       }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

## <a name="custom-roles"></a>Funções personalizadas

Se você precisar especificar permissões que são diferentes de funções internas, você pode criar funções personalizadas.

A seguir estão definições de função da Grade de Eventos de exemplo que permitem aos usuários executar diferentes ações. Essas funções personalizadas são diferentes das funções internas porque elas concedem acesso mais amplo do que apenas assinaturas de eventos.

**EventGridReadOnlyRole.json**: permitir apenas operações somente leitura.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.json**: permitir ações restritas posteriores, mas impedir ações de exclusão.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.json**: permitir todas as ações da grade de eventos.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

Você pode criar funções personalizadas com [PowerShell](../role-based-access-control/custom-roles-powershell.md), [Azure CLI](../role-based-access-control/custom-roles-cli.md) e [REST](../role-based-access-control/custom-roles-rest.md).



### <a name="encryption-at-rest"></a>Criptografia em repouso

Todos os eventos ou dados gravados no disco pelo serviço de grade de eventos são criptografados por uma chave gerenciada pela Microsoft, garantindo que ele seja criptografado em repouso. Além disso, o período máximo de tempo que os eventos ou os dados retidos é de 24 horas em conformidade com a [política de repetição de grade de eventos](delivery-and-retry.md). A grade de eventos excluirá automaticamente todos os eventos ou dados após 24 horas ou a vida útil do evento, o que for menor.

## <a name="next-steps"></a>Próximas etapas

* Para obter uma introdução à grade de eventos, consulte [sobre a grade de eventos](overview.md)
