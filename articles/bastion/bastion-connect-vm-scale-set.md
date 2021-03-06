---
title: Conectar-se a um conjunto de dimensionamento de máquinas virtuais do Windows usando a bastiões do Azure | Microsoft Docs
description: Neste artigo, saiba como se conectar a um conjunto de dimensionamento de máquinas virtuais do Azure usando a bastiões do Azure.
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: charwen
ms.openlocfilehash: 290a20fcd827841c24983f3bdd54b6db8e154462
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80619335"
---
# <a name="connect-to-a-virtual-machine-scale-set-using-azure-bastion"></a>Conectar-se a um conjunto de dimensionamento de máquinas virtuais usando a bastiões do Azure

Este artigo mostra como usar o RDP de forma segura e direta para a instância do conjunto de dimensionamento de máquinas virtuais do Windows em uma rede virtual do Azure usando a bastiões do Azure. Você pode se conectar a uma instância do conjunto de dimensionamento de máquinas virtuais diretamente da portal do Azure. Ao usar a bastiões do Azure, as VMs não exigem um cliente, agente ou software adicional. Para obter mais informações sobre a bastiões do Azure, consulte a [visão geral](bastion-overview.md).

## <a name="before-you-begin"></a>Antes de começar

Verifique se você configurou um host de bastiões do Azure para a rede virtual na qual o conjunto de dimensionamento de máquinas virtuais reside. Para obter mais informações, consulte [criar um host de bastiões do Azure](bastion-create-host-portal.md). Depois que o serviço de bastiões for provisionado e implantado em sua rede virtual, você poderá usá-lo para se conectar a uma instância do conjunto de dimensionamento de máquinas virtuais nessa rede virtual. A bastiões pressupõe que você esteja usando o RDP para se conectar a um conjunto de dimensionamento de máquinas virtuais do Windows e o SSH para se conectar ao conjunto de dimensionamento de máquinas virtuais do Linux. Para obter informações sobre a conexão a uma VM do Linux, consulte [conectar-se a uma VM-Linux](bastion-connect-vm-ssh.md).

## <a name="connect-using-rdp"></a><a name="rdp"></a>Conectar usando RDP

1. Abra o [portal do Azure](https://portal.azure.com). Navegue até o conjunto de dimensionamento de máquinas virtuais ao qual você deseja se conectar.

   ![navigate](./media/bastion-connect-vm-scale-set/1.png)
2. Navegue até a instância do conjunto de dimensionamento de máquinas virtuais à qual você deseja se conectar e, em seguida, selecione **conectar**. Ao usar uma conexão RDP, o conjunto de dimensionamento de máquinas virtuais deve ser um conjunto de dimensionamento de máquinas virtuais do Windows.

   ![conjunto de dimensionamento de máquinas virtuais](./media/bastion-connect-vm-scale-set/2.png)
3. Depois de selecionar **conectar**, uma barra lateral aparece com três guias – RDP, SSH e bastiões. Selecione a guia **bastião** na barra lateral. Se você não provisionar a bastiões para a rede virtual, poderá selecionar o link para configurar a bastiões. Para obter instruções de configuração, consulte [Configurar a bastiões](bastion-create-host-portal.md).

   ![Guia de bastiões](./media/bastion-connect-vm-scale-set/3.png)
4. Na guia bastião, insira o nome de usuário e a senha para o conjunto de dimensionamento de máquinas virtuais e, em seguida, selecione **conectar**.

   ![connect](./media/bastion-connect-vm-scale-set/4.png)
5. A conexão RDP com essa máquina virtual via bastiões será aberta diretamente no portal do Azure (sobre o HTML5) usando a porta 443 e o serviço de bastiões.

## <a name="next-steps"></a>Próximas etapas

Leia as [perguntas frequentes sobre bastiões](bastion-faq.md).