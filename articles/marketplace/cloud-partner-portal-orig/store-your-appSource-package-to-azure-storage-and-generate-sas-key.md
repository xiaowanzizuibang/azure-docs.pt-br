---
title: Armazenar o pacote do AppSource no armazenamento do Azure e gerar uma URL com chave SAS
description: Detalhes das etapas necessárias para carregar e proteger um pacote do AppSource.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: dsindona
ms.openlocfilehash: 57bc370fd160b8b3d6d7941ea28cd460c99f3d96
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80285360"
---
<a name="store-your-appsource-package-to-azure-storage-and-generate-a-url-with-sas-key"></a>Armazenar o pacote do AppSource no armazenamento do Azure e gerar uma URL com chave SAS
=============================================================================

Para manter a segurança dos arquivos, todos os parceiros devem armazenar o arquivo do pacote do AppSource em uma conta de armazenamento de blobs do Azure e usar uma chave SAS para compartilhá-lo. Recuperaremos o arquivo do pacote do local de armazenamento do Azure para certificação e usá-lo para testes do AppSource.

Use as etapas a seguir para carregar o pacote no armazenamento de blobs:

1. Vá para <https://azure.microsoft.com> e crie uma conta de avaliação gratuita ou cobrada.

2. Entre no [portal do Azure](https://portal.azure.com/).

3. Crie uma nova conta de armazenamento, clicando em **+ Novo** e indo para a conta **Dados + Armazenamento**.

   ![Folha Dados + Armazenamento no portal do Microsoft Azure](media/CRMScreenShot7.png)

4. Insira um **Nome** e o nome do **Grupo de Recursos** e clique no botão **Criar**.

   ![Criar uma conta de armazenamento no portal do Microsoft Azure](media/CRMScreenShot8.png)

5. Navegue até o grupo de recursos criado recentemente e crie um novo contêiner de blob.

   ![Carregar pacote como blob usando o portal do Microsoft Azure](media/CRMScreenShot9.png)

6. Se você ainda não fez isso, baixe e instale o [Gerenciador de Armazenamento do Microsoft Azure](https://storageexplorer.com/).

7. Abra o Gerenciador de Armazenamento e use o ícone para conectar-se à conta de armazenamento do Azure.

8. Navegue até o contêiner de blob que você criou e clique em **Upload** para adicionar o arquivo zip do pacote.

   ![Carregar o pacote usando o Gerenciador de Armazenamento do Microsoft Azure](media/CRMScreenShot10.png)

9. Clique com o botão direito do mouse no seu arquivo e selecione **Obter assinatura de acesso compartilhado**.

   ![Obter assinatura de acesso compartilhado de um arquivo do Azure](media/CRMScreenShot11.png)

10. Modifique o **Tempo de Expiração** para ativar a SAS por um mês e clique em **Criar**.

    ![Modificar a data de expiração de SAS de um arquivo do Azure](media/CRMScreenShot12.png)

11. Copie o campo da URL e salve-o para mais tarde. Essa URL será necessária quando você criar a oferta associada. 

    ![Copia a URL de SAS de um arquivo do Azure](media/CRMScreenShot13.png)

