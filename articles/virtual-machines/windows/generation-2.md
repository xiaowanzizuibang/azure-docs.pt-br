---
title: Suporte do Azure para VMs de geração 2
description: Visão geral do suporte do Azure para VMs de geração 2
author: ju-shim
ms.service: virtual-machines
ms.subservice: sizes
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 02/11/2020
ms.author: jushiman
ms.openlocfilehash: 07bf03096cbe5ec8e0fe9619519fdddb91cde984
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82231655"
---
# <a name="support-for-generation-2-vms-on-azure"></a>Suporte para VMs de geração 2 no Azure

O suporte para VMs (máquinas virtuais) de geração 2 agora está disponível no Azure. Você não pode alterar a geração de uma máquina virtual depois de criá-la, portanto, examine as considerações nesta página antes de escolher uma geração.

As VMs de geração 2 oferecem suporte a recursos principais que não têm suporte em VMs de geração 1. Esses recursos incluem memória aumentada, extensões Intel SGX (Intel® software Guard) e vPMEM (memória persistente virtualizada). As VMs de geração 2 em execução no local têm alguns recursos que ainda não têm suporte no Azure. Para obter mais informações, consulte a seção [recursos e funcionalidades](#features-and-capabilities) .

As VMs de geração 2 usam a nova arquitetura de inicialização baseada em UEFI em vez da arquitetura baseada em BIOS usada pelas VMs de geração 1. Em comparação com as VMs de geração 1, as VMs de geração 2 podem ter tempos de inicialização e de instalação aprimorados. Para obter uma visão geral das VMs de geração 2 e algumas das diferenças entre a geração 1 e a geração 2, consulte [devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

## <a name="generation-2-vm-sizes"></a>Tamanhos de VM de geração 2

As VMs de geração 1 têm suporte de todos os tamanhos de VM no Azure (exceto para VMs da série Mv2). O Azure agora oferece suporte à geração 2 para a seguinte série de VMs selecionada:

* [Série B](https://docs.microsoft.com/azure/virtual-machines/windows/b-series-burstable)
* [Série DCsv2](../dcv2-series.md)
* Série [DSv2](../dv2-dsv2-series.md) e [série Dsv3](../dv3-dsv3-series.md)
* [Série Dasv4](https://docs.microsoft.com/azure/virtual-machines/dav4-dasv4-series)
* [Série Esv3](../ev3-esv3-series.md)
* [Série Easv4](https://docs.microsoft.com/azure/virtual-machines/eav4-easv4-series)
* [Série Fsv2](../fsv2-series.md)
* [Série GS](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#gs-series)
* [Série HB](../hb-series.md)
* [Série HC](../hc-series.md)
* Série [ls](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#ls-series) e [série Lsv2](../lsv2-series.md)
* [Série Mv2](../mv2-series.md)<sup>1</sup>
* Série [NCv2](../ncv2-series.md) e [série NCv3](../ncv3-series.md)
* [Série ND](../nd-series.md)
* [Série NVv3](../nvv3-series.md)

<sup>1</sup> a série Mv2 não dá suporte a imagens de VM de geração 1 e só oferece suporte a um subconjunto de imagens de geração 2. Consulte [a documentação da série Mv2](https://docs.microsoft.com/azure/virtual-machines/mv2-series) para obter detalhes.

## <a name="generation-2-vm-images-in-azure-marketplace"></a>Imagens de VM de geração 2 no Azure Marketplace

As VMs de geração 2 dão suporte às seguintes imagens do Marketplace:

* Windows Server 2019, 2016, 2012 R2, 2012
* Windows 10 Pro, Windows 10 Enterprise
* SUSE Linux Enterprise Server 15 SP1
* SUSE Linux Enterprise Server 12 SP4
* Ubuntu Server 16, 4, 18, 4, 19, 4, 19,10 
* RHEL 8,1, 8,0, 7,7, 7,6, 7,5, 7,4, 7,0
* Cent OS 8,1, 8,0, 7,7, 7,6, 7,5, 7,4
* Oracle Linux 7,7, 7,7-CI

> [!NOTE]
> Tamanhos de máquina virtual específicos, como a série Mv2, podem dar suporte apenas a um subconjunto dessas imagens – consulte a documentação de tamanho da máquina virtual relevante para obter detalhes completos.

## <a name="on-premises-vs-azure-generation-2-vms"></a>VMs locais vs. Azure geração 2

Atualmente, o Azure não dá suporte a alguns dos recursos que o Hyper-V local dá suporte para VMs de geração 2.

| Recurso de geração 2                | Hyper-V local | Azure |
|-------------------------------------|---------------------|-------|
| Inicialização segura                         | :heavy_check_mark:  | :x:   |
| VM blindada                         | :heavy_check_mark:  | :x:   |
| vTPM                                | :heavy_check_mark:  | :x:   |
| Segurança baseada em virtualização (VBS) | :heavy_check_mark:  | :x:   |
| Formato VHDX                         | :heavy_check_mark:  | :x:   |

## <a name="features-and-capabilities"></a>Recursos e funcionalidades

### <a name="generation-1-vs-generation-2-features"></a>Recursos da geração 1 versus geração 2

| Recurso | Geração 1 | 2ª geração |
|---------|--------------|--------------|
| Inicialização             | PCAT                      | UEFI                               |
| Controladores de disco | IDE                       | SCSI                               |
| Tamanhos de VM         | Todos os tamanhos de VM | Somente VMs que dão suporte ao armazenamento Premium |

### <a name="generation-1-vs-generation-2-capabilities"></a>Recursos de geração 1 versus geração 2

| Funcionalidade | Geração 1 | 2ª geração |
|------------|--------------|--------------|
| Disco do sistema operacional > 2 TB                    | :x:                | :heavy_check_mark: |
| Disco personalizado/imagem/sistema operacional de permuta         | :heavy_check_mark: | :heavy_check_mark: |
| Suporte ao conjunto de dimensionamento de máquinas virtuais | :heavy_check_mark: | :heavy_check_mark: |
| Azure Site Recovery               | :heavy_check_mark: | :heavy_check_mark: |
| Backup/restauração                    | :heavy_check_mark: | :heavy_check_mark: |
| Galeria de imagens compartilhadas              | :heavy_check_mark: | :heavy_check_mark: |
| Criptografia de disco do Azure             | :heavy_check_mark: | :x:                |

## <a name="creating-a-generation-2-vm"></a>Criando uma VM de geração 2

### <a name="marketplace-image"></a>Imagem do Marketplace

No portal do Azure ou CLI do Azure, você pode criar VMs de geração 2 de uma imagem do Marketplace que dá suporte à inicialização de UEFI.

#### <a name="azure-portal"></a>Portal do Azure

Abaixo estão as etapas para criar uma VM de geração 2 (Gen2) no portal do Azure.

1. Entre no Portal do Azure em https://portal.azure.com.
1. Selecione **Criar um recurso**.
1. Clique em **ver tudo** no Azure Marketplace à esquerda.
1. Selecione uma imagem que dê suporte a Gen2.
1. Clique em **Criar**.
1. Na guia **avançado** , na seção **geração de VM** , selecione a opção **Gen 2** .
1. Na guia **noções básicas** , em **detalhes da instância**, vá para **tamanho** e abra a folha **selecionar um tamanho da VM** .
1. Selecione uma [VM de geração 2 com suporte](#generation-2-vm-sizes).
1. Percorra o [fluxo de criação de portal do Azure](quick-create-portal.md) para concluir a criação da VM.

![Selecione a VM Gen 1 ou Gen 2](./media/generation-2/gen1-gen2-select.png)

#### <a name="powershell"></a>PowerShell

Você também pode usar o PowerShell para criar uma VM referenciando diretamente a SKU de geração 1 ou geração 2.

Por exemplo, use o seguinte cmdlet do PowerShell para obter uma lista das SKUs na `WindowsServer` oferta.

```powershell
Get-AzVMImageSku -Location westus2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```

Como alternativa, você pode usar o CLI do Azure para ver quaisquer imagens disponíveis da geração 2, listadas pelo **Publicador**.

```azurecli
az vm image list --publisher Canonical --sku gen2 --output table --all
```

Se você estiver criando uma VM com o Windows Server 2012 como o sistema operacional, você selecionará a SKU de VM de geração 1 (BIOS) ou de geração 2 (UEFI), que tem esta aparência:

```powershell
2012-Datacenter
2012-datacenter-gensecond
```

Consulte a seção [recursos e funcionalidades](#features-and-capabilities) para obter uma lista atual de imagens do Marketplace com suporte.

### <a name="managed-image-or-managed-disk"></a>Imagem gerenciada ou disco gerenciado

Você pode criar uma VM de geração 2 de uma imagem gerenciada ou de um disco gerenciado da mesma maneira que criaria uma VM de geração 1.

### <a name="virtual-machine-scale-sets"></a>Conjuntos de dimensionamento de máquinas virtuais

Você também pode criar VMs de geração 2 usando conjuntos de dimensionamento de máquinas virtuais. No CLI do Azure, use conjuntos de dimensionamento do Azure para criar VMs de geração 2.

## <a name="frequently-asked-questions"></a>Perguntas frequentes

* **As VMs de geração 2 estão disponíveis em todas as regiões do Azure?**  
    Sim. Mas nem todos os [tamanhos de VM de geração 2](#generation-2-vm-sizes) estão disponíveis em todas as regiões. A disponibilidade da VM de geração 2 depende da disponibilidade do tamanho da VM.

* **Há uma diferença de preço entre as VMs de geração 1 e de geração 2?**  
   Não.

* **Tenho um arquivo. vhd da minha VM de geração 2 local. Posso usar esse arquivo. VHD para criar uma VM de geração 2 no Azure?**
  Sim, você pode colocar seu arquivo. VHD de geração 2 no Azure e usá-lo para criar uma VM de geração 2. Use as seguintes etapas para fazer isso:
    1. Carregue o. VHD em uma conta de armazenamento na mesma região em que você gostaria de criar sua VM.
    1. Crie um disco gerenciado com base no arquivo. vhd. Defina a propriedade de geração do Hyper-V como v2. Os comandos do PowerShell a seguir definem a propriedade de geração do Hyper-V ao criar um disco gerenciado.

        ```powershell
        $sourceUri = 'https://xyzstorage.blob.core.windows.net/vhd/abcd.vhd'. #<Provide location to your uploaded .vhd file>
        $osDiskName = 'gen2Diskfrmgenvhd'  #<Provide a name for your disk>
        $diskconfig = New-AzDiskConfig -Location '<location>' -DiskSizeGB 127 -AccountType Standard_LRS -OsType Windows -HyperVGeneration "V2" -SourceUri $sourceUri -CreateOption 'Import'
        New-AzDisk -DiskName $osDiskName -ResourceGroupName '<Your Resource Group>' -Disk $diskconfig
        ```

    1. Quando o disco estiver disponível, crie uma VM anexando esse disco. A VM criada será uma VM de geração 2.
    Quando a VM de geração 2 é criada, você pode, opcionalmente, generalizar a imagem dessa VM. Ao generalizar a imagem, você pode usá-la para criar várias VMs.

* **Como fazer aumentar o tamanho do disco do sistema operacional?**  
  OS discos do sistema operacional com mais de 2 TB são novos para VMs de geração 2. Por padrão, OS discos do sistema operacional são menores que 2 TB para VMs de geração 2. Você pode aumentar o tamanho do disco até um máximo recomendado de 4 TB. Use o CLI do Azure ou o portal do Azure para aumentar o tamanho do disco do sistema operacional. Para obter informações sobre como expandir discos programaticamente, consulte [redimensionar um disco](expand-os-disk.md).

  Para aumentar o tamanho do disco do sistema operacional do portal do Azure:

  1. Na portal do Azure, vá para a página de propriedades da VM.
  1. Para desligar e desalocar a VM, selecione o botão **parar** .
  1. Na seção **discos** , selecione o disco do sistema operacional que você deseja aumentar.
  1. Na seção **discos** , selecione **configuração**e atualize o **tamanho** para o valor desejado.
  1. Volte para a página de propriedades da VM e **inicie** a VM.
  
  Você pode ver um aviso para discos do sistema operacional com mais de 2 TB. O aviso não se aplica às VMs de geração 2. No entanto, OS tamanhos de disco do sistema operacional maiores que 4 TB *não são recomendados.*

* **As VMs de geração 2 dão suporte à rede acelerada?**  
    Sim. Para obter mais informações, consulte [criar uma VM com rede acelerada](../../virtual-network/create-vm-accelerated-networking-cli.md).

* **Há suporte para VHDX na geração 2?**  
    Não, as VMs de geração 2 dão suporte apenas a VHD.

* **As VMs de geração 2 dão suporte ao Azure ultra Armazenamento em Disco?**  
    Sim.

* **Posso migrar uma VM da geração 1 para a geração 2?**  
    Não, você não pode alterar a geração de uma VM depois de criá-la. Se você precisar alternar entre gerações de VM, crie uma nova VM de uma geração diferente.

* **Por que o tamanho da minha VM não está habilitado no seletor de tamanho quando tento criar uma VM Gen2?**

    Isso pode ser resolvido fazendo o seguinte:

    1. Verifique se a propriedade de **geração de VM** está definida como **Gen 2** na guia **avançado** .
    1. Verifique se você está procurando um [tamanho de VM que dá suporte a VMs Gen2](#generation-2-vm-sizes).

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre as [máquinas virtuais de geração 2 no Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

* Saiba como [preparar um VHD](prepare-for-upload-vhd-image.md) para carregar de sistemas locais para o Azure.
