---
title: Solucionar problemas Área de Trabalho Remota área de trabalho virtual do Windows cliente-Azure
description: Como resolver problemas ao configurar conexões de cliente em um ambiente de locatário de área de trabalho virtual do Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 03/31/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 595762e6e8f22dddff30f1cff8c4bb79e89624b1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80473855"
---
# <a name="troubleshoot-the-remote-desktop-client"></a>Solucionar problemas do cliente do Área de Trabalho Remota

Este artigo descreve problemas comuns com o cliente Área de Trabalho Remota e como corrigi-los.

## <a name="remote-desktop-client-for-windows-7-or-windows-10-stops-responding-or-cannot-be-opened"></a>Área de Trabalho Remota cliente para Windows 7 ou Windows 10 para de responder ou não pode ser aberto

A partir da versão 1.2.790, você pode redefinir os dados do usuário da página sobre ou usando um comando.

Use o comando a seguir para remover os dados do usuário, restaurar as configurações padrão e cancelar a assinatura de todos os espaços de trabalho.

```cmd
msrdcw.exe /reset [/f]
```

Se você estiver usando uma versão anterior do cliente Área de Trabalho Remota, recomendamos desinstalar e reinstalar o cliente.

## <a name="web-client-wont-open"></a>O cliente Web não será aberto

Primeiro, teste sua conexão com a Internet abrindo outro site em seu navegador; por exemplo, [www.Bing.com](https://www.bing.com).

Use **nslookup** para confirmar se o DNS pode resolver o FQDN:

```cmd
nslookup rdweb.wvd.microsoft.com
```

Tente se conectar com outro cliente, como Área de Trabalho Remota Client para Windows 7 ou Windows 10, e verifique se você pode abrir o cliente Web.

### <a name="opening-another-site-fails"></a>Falha ao abrir outro site

Isso geralmente é causado por problemas de conexão de rede ou por uma interrupção de rede. Recomendamos que você entre em contato com o suporte de rede.

### <a name="nslookup-cannot-resolve-the-name"></a>Nslookup não pode resolver o nome

Isso geralmente é causado por problemas de conexão de rede ou por uma interrupção de rede. Recomendamos que você entre em contato com o suporte de rede.

### <a name="your-client-cant-connect-but-other-clients-on-your-network-can-connect"></a>O cliente não pode se conectar, mas outros clientes em sua rede podem se conectar

Se seu navegador começar a funcionar ou parar de trabalhar enquanto você estiver usando o cliente Web, siga estas instruções para solucionar o problema:

1. Reinicie o navegador.
2. Limpar cookies do navegador. Consulte [como excluir arquivos de cookie no Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
3. Limpe o cache do navegador. Consulte [Limpar cache do navegador para seu navegador](https://binged.it/2RKyfdU).
4. Abra o navegador em modo privado.

## <a name="web-client-stops-responding-or-disconnects"></a>O cliente Web para de responder ou desconexões

Tente se conectar usando outro navegador ou cliente.

### <a name="other-browsers-and-clients-also-malfunction-or-fail-to-open"></a>Outros navegadores e clientes também têm mau funcionamento ou falha ao abrir

Se os problemas continuarem mesmo depois de você ter alternado os navegadores, o problema poderá não ser com o navegador, mas com sua rede. Recomendamos que você entre em contato com o suporte de rede.

## <a name="web-client-keeps-prompting-for-credentials"></a>O cliente Web continua solicitando credenciais

Se o cliente Web continuar solicitando credenciais, siga estas instruções:

1. Confirme se a URL do cliente Web está correta.
2. Confirme se as credenciais que você está usando são para o ambiente de área de trabalho virtual do Windows vinculado à URL.
3. Limpar cookies do navegador. Para obter mais detalhes, consulte [como excluir arquivos de cookie no Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
4. Limpe o cache do navegador. Para obter mais detalhes, consulte [Limpar cache do navegador para seu navegador](https://binged.it/2RKyfdU).
5. Abra seu navegador em modo privado.

## <a name="next-steps"></a>Próximas etapas

- Para obter uma visão geral da solução de problemas da área de trabalho virtual do Windows e das faixas de escalonamento, consulte [visão geral da solução de problemas, comentários e suporte](troubleshoot-set-up-overview.md).
- Para solucionar problemas ao criar um pool de locatários e de host em um ambiente de área de trabalho virtual do Windows, confira [criação de locatário e pool de hosts](troubleshoot-set-up-issues.md).
- Para solucionar problemas durante a configuração de uma VM (máquina virtual) na área de trabalho virtual do Windows, consulte [configuração de máquina virtual do host de sessão](troubleshoot-vm-configuration.md).
- Para solucionar problemas ao usar o PowerShell com a área de trabalho virtual do Windows, consulte [PowerShell da área de trabalho virtual do Windows](troubleshoot-powershell.md).
- Para percorrer um tutorial de solução de problemas, consulte [tutorial: solucionar problemas de implantações de modelo do Resource Manager](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).