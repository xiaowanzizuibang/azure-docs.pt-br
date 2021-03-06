---
title: Como solucionar problemas do catálogo de dados do Azure
description: Este artigo descreve as preocupações comuns de solução de problemas para recursos do catálogo de dados do Azure.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: troubleshooting
ms.date: 08/01/2019
ms.openlocfilehash: 84bd14f8ae18527b4f6e9d8509a12555baec8771
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "68879549"
---
# <a name="troubleshooting-azure-data-catalog"></a>Solução de problemas do Catálogo de Dados do Azure

Este artigo descreve as preocupações comuns de solução de problemas para recursos do catálogo de dados do Azure. 

## <a name="functionality-limitations"></a>Limitações de funcionalidade

Ao usar o catálogo de dados do Azure, a seguinte funcionalidade é limitada:

- Não há suporte para contas com **função de convidado** de tipo. Você não pode adicionar contas de convidado como usuários do catálogo de dados do Azure e os usuários convidados não [https://www.azuredatacatalog.com](https://www.azuredatacatalog.com)podem usar o portal em.

- Não há suporte para a criação de recursos do catálogo de dados do Azure usando modelos de Azure Resource Manager ou comandos Azure PowerShell.

- O recurso do catálogo de dados do Azure não pode ser movido entre locatários do Azure.

## <a name="azure-active-directory-policy-configuration"></a>Configuração de política do Azure Active Directory

Pode haver uma situação em que você pode entrar no portal do Catálogo de Dados do Azure, mas ao tentar entrar na ferramenta de registro de fonte de dados, poderá receber uma mensagem de erro impedindo a entrada. Esse erro pode ocorrer quando você está na rede da empresa ou quando você está se conectando de fora da rede da empresa.

A ferramenta de registro usa a *Autenticação de Formulários* para validar logons de usuário no Azure Active Directory. Para entrar com êxito, um administrador do Azure Active Directory deve habilitar a autenticação de formulários na *política de autenticação global*.

Com a política de autenticação global, você pode habilitar a autenticação separadamente para conexões de intranet e extranet, conforme mostrado na imagem a seguir. Erros de entrada poderão ocorrer se a autenticação de formulários não estiver habilitada para a rede da qual você está se conectando.

 ![Política de Autenticação Global do AzureActive Directory](./media/troubleshoot-policy-configuration/global-auth-policy.png)

Para saber mais, confira [Configurando políticas de autenticação](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486781(v=ws.11)).

## <a name="next-steps"></a>Próximas etapas

* [Criar um Catálogo de Dados do Azure](data-catalog-get-started.md)
