---
title: Salvar e distribuir imagens no Azure DevTest Labs | Microsoft Docs
description: Este artigo fornece as etapas para salvar imagens personalizadas das VMs (máquinas virtuais) já criadas no Azure DevTest Labs.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: e5bc8e5041bfe6d95e3ff1a93bb3338ccead5bb4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76759424"
---
# <a name="save-custom-images-and-distribute-to-multiple-labs"></a>Salvar imagens personalizadas e distribuí-las para vários laboratórios
Este artigo fornece as etapas para salvar imagens personalizadas das VMs (máquinas virtuais) já criadas. Ele também aborda como distribuir essas imagens personalizadas para outros laboratórios do DevTest na organização.

## <a name="prerequisites"></a>Pré-requisitos
Os itens a seguir já devem estar em vigor:

- Um laboratório para a fábrica de imagens no Azure DevTest Labs.
- Um projeto DevOps do Azure que é usado para automatizar a fábrica de imagens.
- Local do código-fonte que contém os scripts e a configuração (em nosso exemplo, no mesmo projeto DevOps mencionado na etapa anterior).
- Criar definição para orquestrar as tarefas do Azure PowerShell.

Se necessário, siga as etapas em [executar uma fábrica de imagens do Azure DevOps](image-factory-set-up-devops-lab.md) para criar ou configurar esses itens. 

## <a name="save-vms-as-generalized-vhds"></a>Salvar VMs como VHDs generalizados
Salve as VMs existentes como VHDs generalizados.  Há um exemplo de script do PowerShell para salvar as VMs existentes como VHDs generalizados. Para usá-lo, primeiro adicione outra tarefa **do Azure PowerShell** à definição de compilação, conforme mostrado na imagem a seguir:

![Adicionar etapa de Azure PowerShell](./media/save-distribute-custom-images/powershell-step.png)

Quando você tiver a nova tarefa na lista, selecione o item para que possamos preencher todos os detalhes, conforme mostrado na imagem a seguir: 

![Configurações do PowerShell](./media/save-distribute-custom-images/powershell-settings.png)


## <a name="generalized-vs-specialized-custom-images"></a>Imagens personalizadas vs. generalizadas e especializadas
No [portal do Azure](https://portal.azure.com), ao criar uma imagem personalizada de uma máquina virtual, você pode optar por ter uma imagem personalizada generalizada ou especializada.

- **Imagem personalizada especializada:** O Sysprep/desprovisionamento não foi executado no computador. Isso significa que a imagem é uma cópia exata do disco do sistema operacional na máquina virtual existente (um instantâneo).  Os mesmos arquivos, aplicativos, contas de usuário, nome do computador e assim por diante estão todos presentes quando criamos um novo computador com base nessa imagem personalizada.
- **Imagem personalizada generalizada:** O Sysprep/desprovisionamento foi executado no computador.  Quando esse processo é executado, ele remove contas de usuário, remove o nome do computador, retira os hives do registro do usuário, etc., com o objetivo de generalizar a imagem para que ela possa ser personalizada durante a criação de outra máquina virtual.  Quando você generaliza uma máquina virtual (executando sysprep), o processo destrói a máquina virtual atual – ela não será mais funcional.

O script para ajustar imagens personalizadas na fábrica de imagens salvará VHDs para qualquer máquina virtual criada na etapa anterior (identificada com base em uma marca no recurso no Azure).

## <a name="update-configuration-for-distributing-images"></a>Atualizar a configuração para distribuir imagens
A próxima etapa do processo é enviar por push as imagens personalizadas do laboratório de fábrica de imagens para todos os outros laboratórios que precisarem delas. A parte principal desse processo é o arquivo de configuração **Labs. JSON** . Você pode encontrar esse arquivo na pasta de **configuração** incluída no Image Factory.

Há duas coisas principais listadas no arquivo de configuração Labs. JSON:

- Identificar exclusivamente um laboratório de destino específico usando a ID da assinatura e o nome do laboratório.
- O conjunto específico de imagens que deve ser enviado por push para o laboratório como caminhos relativos à raiz de configuração. Você também pode especificar a pasta inteira (para obter todas as imagens nessa pasta).

Aqui está um exemplo de arquivo Labs. JSON com dois laboratórios listados. Nesse caso, você está distribuindo imagens para dois laboratórios diferentes.

```json
{
   "Labs": [
      {
         "SubscriptionId": "<subscription ID that contains the lab>",
         "LabName": "<Name of the DevTest Lab>",
         "ImagePaths": [
               "Win2012R2",
               "Win2016/Datacenter.json"
         ]
      },
      {
         "SubscriptionId": "<subscription ID that contains the lab>",
         "LabName": "<Name of the DevTest Lab>",
         "ImagePaths": [
               "Win2016/Datacenter.json"
         ]
      }
   ]
}
```

## <a name="create-a-build-task"></a>Criar uma tarefa de build
Usando as mesmas etapas que você viu anteriormente neste artigo, adicione uma tarefa de compilação **do Azure PowerShell** adicional para criar a definição. Preencha os detalhes conforme mostrado na imagem a seguir: 

![Compilar tarefa para distribuir imagens](./media/save-distribute-custom-images/second-build-task-powershell.png)

Os parâmetros são:`-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -SubscriptionId $(SubscriptionId) -DevTestLabName $(DevTestLabName) -maxConcurrentJobs 20`

Essa tarefa usa qualquer imagem personalizada presente na fábrica de imagens e as envia para os laboratórios definidos no arquivo Labs. JSON.

## <a name="queue-the-build"></a>Enfileirar a compilação
Depois que a tarefa de compilação de distribuição for concluída, enfileirar uma nova compilação para certificar-se de que tudo está funcionando. Depois que a compilação for concluída com êxito, as novas imagens personalizadas aparecerão no laboratório de destino inserido no arquivo de configuração Labs. JSON.

## <a name="next-steps"></a>Próximas etapas
No próximo artigo da série, você atualiza a fábrica de imagens com uma política de retenção e etapas de limpeza: [definir política de retenção e executar scripts de limpeza](image-factory-set-retention-policy-cleanup.md).
