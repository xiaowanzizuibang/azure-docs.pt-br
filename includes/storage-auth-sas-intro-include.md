---
title: incluir arquivo
description: incluir arquivo
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/20/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: aec1faa4de1149f08fb6fbc1cc5bf3aa2ab6becd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/27/2020
ms.locfileid: "75371762"
---
Uma SAS (assinatura de acesso compartilhado) permite que você conceda acesso limitado a contêineres e blobs em sua conta de armazenamento. Ao criar uma SAS, você especifica suas restrições, incluindo quais recursos de armazenamento do Azure um cliente tem permissão para acessar, quais permissões eles têm nesses recursos e por quanto tempo a SAS é válida.

Cada SAS é assinada com uma chave. Você pode assinar uma SAS de uma das duas maneiras:

- Com uma chave criada usando as credenciais do Azure Active Directory (AD do Azure). Uma SAS que é assinada com as credenciais do Azure AD é uma SAS de *delegação de usuário* .
- Com a chave da conta de armazenamento. Uma *SAS de serviço* e uma *SAS de conta* são assinadas com a chave da conta de armazenamento.

Uma SAS de delegação de usuário oferece segurança superior a uma SAS que é assinada com a chave da conta de armazenamento. A Microsoft recomenda usar uma SAS de delegação de usuário quando possível. Para obter mais informações, consulte [conceder acesso limitado a dados com SAS (assinaturas de acesso compartilhado)](../articles/storage/common/storage-sas-overview.md).
