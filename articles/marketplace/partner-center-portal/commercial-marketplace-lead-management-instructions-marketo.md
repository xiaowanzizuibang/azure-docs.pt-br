---
title: Configurar gerenciamento de leads no Marketo | Azure Marketplace
description: Configure o gerenciamento de leads para clientes do Marketo para o Azure Marketplace.
author: qianw211
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.openlocfilehash: b142e0ab1aaa242157e207ceecf958be51bb1721
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82133648"
---
# <a name="configure-lead-management-in-marketo"></a>Configurar o gerenciamento de cliente potencial no Marketo

Este artigo descreve como configurar seu sistema CRM do Marketo para processar os leads de vendas de sua oferta de Marketplace comercial.

## <a name="set-up-your-marketo-crm-system"></a>Configurar seu sistema de CRM do Marketo

1. Entre no Marketo.

1. Selecione **Design Studio**.

    ![Marketo Design Studio](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-1.png)

1.  Selecione **Novo Formulário**.

    ![Novo formulário do marketo](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-2.png)

1.  Preencha os campos obrigatórios na caixa de diálogo **novo formulário** e, em seguida, selecione **criar**.

    ![Criar novo formulário do Marketo](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-3.png)

1.  Na página **detalhes do campo** , selecione **concluir**.

    ![Formulário de conclusão do Marketo](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-4.png)

1.  Aprove e feche.

1. Na guia **MarketplaceLeadBackend** , selecione **código de inserção**. 

    ![Código de inserção do marketo](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-6.png)

1. O Código de Inserção do Marketo exibe um código semelhante ao exemplo a seguir.

    ```html
    <form id="mktoForm_1179"></form>
    <script>MktoForms2.loadForm("("//app-ys12.marketo.com", "123-PQR-789", 1179);</script>
    ```

1. Copie os valores para os campos a seguir mostrados no formulário de código de inserção. Você usará esses valores para configurar sua oferta para receber leads na próxima etapa. Use o exemplo a seguir como um guia para obter as IDs necessárias do exemplo de código de inserção do Marketo.

    - ID do servidor = **ys12**
    - ID de Munchkin = **123-PQR-789**
    - ID do formulário = **1179**

    Outra maneira de descobrir esses valores:

    - A ID do servidor é encontrada na URL da sua instância do Marketo, por `serverID.marketo.com`exemplo,.
    - Obtenha a ID do Munchkin da sua assinatura acessando o menu do **administrador** > **Munchkin** no campo **ID da conta do Munchkin** ou da primeira parte do subdomínio do host da API REST `https://{Munchkin ID}.mktorest.com`do marketo:.
    - ID de formulário é a ID do formulário de código de inserção criado na etapa 7 para rotear clientes potenciais do Marketplace.

## <a name="configure-your-offer-to-send-leads-to-marketo"></a>Configurar sua oferta para enviar clientes potenciais ao Marketo

Quando você estiver pronto para configurar as informações de gerenciamento de Lead para sua oferta no portal de publicação, siga estas etapas. 

1. Vá para a página de **instalação da oferta** de sua oferta.

1. Selecione **conectar** na seção **Gerenciamento de leads** . 

    ![Botão conectar da seção Gerenciamento de leads](./media/commercial-marketplace-lead-management-instructions-marketo/lead-management-connect.png)

1. Na janela pop-up **detalhes da conexão** , selecione **marketo** para o **destino do cliente potencial**.

    ![Escolher um destino de cliente potencial](./media/commercial-marketplace-lead-management-instructions-marketo/choose-lead-destination.png)

1. Forneça a **ID do servidor**, a **ID da conta do MUNCHKIN**e a **ID do formulário**.

    > [!NOTE]
    > Você deve concluir a configuração do restante da oferta e publicá-la antes de receber clientes potenciais para a oferta. 

1. Em **email de contato**, insira os endereços de email para as pessoas em sua empresa que devem receber notificações por email quando um novo cliente potencial for recebido. Você pode fornecer vários endereços de email separando-os com um ponto e vírgula.

1. Selecione **OK**.

   Para verificar se você se conectou com êxito a um destino de cliente potencial, selecione o botão **validar** . Se for bem-sucedido, você terá um líder de teste no destino do cliente potencial.

   ![Janela pop-up detalhes da conexão](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-connection-details.png)
