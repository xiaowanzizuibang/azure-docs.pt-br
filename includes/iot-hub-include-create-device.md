---
title: incluir arquivo
description: incluir arquivo
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 11/06/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 7b022f71e197c5695876f2049ee376c3616afc6d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "70049048"
---
<!-- put the ## header in the file that includes this file -->

Nesta seção, você cria uma identidade de dispositivo no registro de identidade no hub IoT. Um dispositivo não pode se conectar a um Hub, a menos que tenha uma entrada no registro de identidade. Para obter mais informações, consulte o [Guia do desenvolvedor do Hub IOT](../articles/iot-hub/iot-hub-devguide-identity-registry.md#identity-registry-operations).

1. No menu de navegação do Hub IoT, abra **dispositivos IOT**e, em seguida, selecione **novo** para adicionar um dispositivo em seu hub IOT.

    ![Criar identidade do dispositivo no portal](./media/iot-hub-include-create-device/create-identity-portal-vs2019.png)

1. Em **criar um dispositivo**, forneça um nome para o novo dispositivo, como **mydeviceid**e selecione **salvar**. Esta ação cria uma identidade de dispositivo para o Hub IoT.

   ![Adicionar um novo dispositivo](./media/iot-hub-include-create-device/create-a-device-vs2019.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. Após a criação do dispositivo, abra o dispositivo na lista do painel **Dispositivos IoT**. Copie a **cadeia de conexão primária** para usar mais tarde.

    ![Cadeia de conexão de dispositivo](./media/iot-hub-include-create-device/device-details-vs2019.png)

> [!NOTE]
> O Registro de identidade do Hub IoT armazena apenas as identidades de dispositivo para habilitar o acesso seguro ao Hub IoT. Ele armazena as IDs e as chaves do dispositivo a usar como credenciais de segurança e um sinalizador habilitado/desabilitado que você poderá usar para desabilitar o acesso de um dispositivo individual. Se seu aplicativo precisar armazenar outros metadados específicos do dispositivo, ele deverá usar um repositório específico do aplicativo. Para saber mais, confira [Guia de Desenvolvedor do Hub IoT](../articles/iot-hub/iot-hub-devguide-identity-registry.md).
