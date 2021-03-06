---
title: Esquemas de evento – IoT Edge de grade de eventos do Azure | Microsoft Docs
description: Esquemas de evento na grade de eventos em IoT Edge.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: ba261aeedf6574f69d3c05f8fd005c912dcc59d1
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/27/2020
ms.locfileid: "73242472"
---
# <a name="event-schemas"></a>Esquemas de evento

O módulo de grade de eventos aceita e entrega eventos no formato JSON. Atualmente, há três esquemas com suporte na grade de eventos:-

* **EventGridSchema**
* **CustomSchema**
* **CloudEventSchema**

Você pode configurar o esquema ao qual um Publicador deve obedecer durante a criação do tópico. Se não for especificado, o padrão será **EventGridSchema**. Eventos que não estão em conformidade com o esquema esperado serão rejeitados.

Os assinantes também podem configurar o esquema no qual desejam os eventos entregues. Se não for especificado, o padrão será o esquema do tópico.
Atualmente, o esquema de entrega de assinante precisa corresponder ao esquema de entrada do tópico. 

## <a name="eventgrid-schema"></a>Esquema EventGrid

O esquema EventGrid consiste em um conjunto de propriedades necessárias às quais uma entidade de publicação deve estar em conformidade. Cada editor precisa preencher os campos de nível superior.

```json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

### <a name="eventgrid-schema-properties"></a>Propriedades do esquema EventGrid

Todos os eventos têm os seguintes dados de nível superior:

| Propriedade | Tipo | Necessária | Descrição |
| -------- | ---- | ----------- |-----------
| topic | cadeia de caracteres | Não | Deve corresponder ao tópico no qual ele está publicado. A grade de eventos popula com o nome do tópico no qual ele é publicado, se não for especificado. |
| subject | cadeia de caracteres | Sim | Caminho definido pelo fornecedor para o assunto do evento. |
| eventType | cadeia de caracteres | Sim | Tipo de evento para essa origem do evento, por exemplo, BlobCreated. |
| eventTime | cadeia de caracteres | Sim | A hora em que o evento é gerado com base na hora UTC do provedor. |
| ID | cadeia de caracteres | Não | Identificador exclusivo do evento. |
| data | objeto | Não | Usado para capturar dados de evento que são específicos para a entidade de publicação. |
| dataVersion | cadeia de caracteres | Sim | A versão do esquema do objeto de dados. O fornecedor define a versão do esquema. |
| metadataVersion | cadeia de caracteres | Não | A versão do esquema do metadados de evento. Grade de Eventos define o esquema de propriedades de nível superior. Grade de Eventos fornece esse valor. |

### <a name="example--eventgrid-schema-event"></a>Exemplo — evento de esquema EventGrid

```json
[
  {
    "id": "1807",
    "eventType": "recordInserted",
    "subject": "myapp/vehicles/motorcycles",
    "eventTime": "2017-08-10T21:03:07+00:00",
    "data": {
      "make": "Ducati",
      "model": "Monster"
    },
    "dataVersion": "1.0"
  }
]
```

## <a name="customevent-schema"></a>Esquema CustomEvent

No esquema personalizado, não há nenhuma propriedade obrigatória que seja imposta como o esquema EventGrid. A entidade de publicação pode controlar totalmente o esquema de evento. Ele fornece flexibilidade máxima e permite cenários em que você já tem um sistema baseado em evento e gostaria de reutilizar os eventos existentes e/ou não quer ser vinculado a um esquema específico.

### <a name="custom-schema-properties"></a>Propriedades do esquema personalizado

Nenhuma propriedade obrigatória. Cabe à entidade de publicação determinar a carga.

### <a name="example--custom-schema-event"></a>Exemplo – evento de esquema personalizado

```json
[
  {
    "eventdata": {
      "make": "Ducati",
      "model": "Monster"
    }
  }
]
```

## <a name="cloudevent-schema"></a>Esquema de CloudEvent

Além dos esquemas acima, a grade de eventos oferece suporte nativo a eventos no [esquema JSON CloudEvents](https://github.com/cloudevents/spec/blob/master/json-format.md). CloudEvents é uma especificação aberta para descrever dados de eventos. Ele simplifica a interoperabilidade fornecendo um esquema de evento comum para a publicação e o consumo de eventos. Ele faz parte do [CNCF](https://www.cncf.io/) e a versão disponível no momento é 1,0-RC1.

### <a name="cloudevent-schema-properties"></a>Propriedades do esquema CloudEvent

Consulte a [especificação CloudEvents](https://github.com/cloudevents/spec/blob/master/json-format.md#3-envelope) nas propriedades de envelope obrigatórias.

### <a name="example--cloud-event"></a>Exemplo – evento de nuvem
```json
[{
       "id": "1807",
       "type": "recordInserted",
       "source": "myapp/vehicles/motorcycles",
       "time": "2017-08-10T21:03:07+00:00",
       "datacontenttype": "application/json",
       "data": {
            "make": "Ducati",
            "model": "Monster"
        },
        "dataVersion": "1.0",
        "specVersion": "1.0-rc1"
}]
```
