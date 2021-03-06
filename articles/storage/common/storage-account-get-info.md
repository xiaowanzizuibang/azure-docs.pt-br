---
title: Obter tipo de conta de armazenamento e nome de SKU com .NET
titleSuffix: Azure Storage
description: Saiba como obter o tipo de conta de armazenamento do Azure e o nome do SKU usando a biblioteca de cliente .NET.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/06/2019
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.openlocfilehash: 1495ed55c24a0f94b911d60d1db0f32940ea134a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79137051"
---
# <a name="get-storage-account-type-and-sku-name-with-net"></a>Obter tipo de conta de armazenamento e nome de SKU com .NET

Este artigo mostra como obter o tipo de conta de armazenamento do Azure e o nome do SKU para um BLOB usando a [biblioteca de cliente de armazenamento do Azure para .net](/dotnet/api/overview/azure/storage?view=azure-dotnet).

As informações de conta estão disponíveis nas versões de serviço que começam com a versão 2018-03-28.

## <a name="about-account-type-and-sku-name"></a>Sobre o tipo de conta e o nome do SKU

**Tipo de conta**: os tipos de `BlobStorage`conta `BlockBlobStorage`válidos `FileStorage`incluem `Storage`,, `StorageV2`, e. A [visão geral da conta de armazenamento do Azure](storage-account-overview.md) tem mais informações, incluindo descrições das várias contas de armazenamento.

**Nome do SKU**: nomes de SKU `Premium_LRS`válidos `Premium_ZRS`incluem `Standard_GRS`, `Standard_GZRS` `Standard_LRS` `Standard_RAGRS` `Standard_RAGZRS`,,,,, `Standard_ZRS`e. Os nomes de SKU diferenciam maiúsculas de minúsculas e são campos de cadeia de caracteres na [classe SkuName](/dotnet/api/microsoft.azure.management.storage.models.skuname?view=azure-dotnet).

## <a name="retrieve-account-information"></a>Recuperar informações da conta

Para obter o tipo de conta de armazenamento e o nome do SKU associado a um blob, chame o método [accountproperties](/dotnet/api/microsoft.azure.storage.blob.cloudblob.getaccountproperties?view=azure-dotnet) ou [GetAccountPropertiesAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.getaccountpropertiesasync?view=azure-dotnet) .

O exemplo de código a seguir recupera e exibe as propriedades da conta somente leitura.

```csharp
private static async Task GetAccountInfoAsync(CloudBlob blob)
{
    try
    {
        // Get the blob's storage account properties.
        AccountProperties acctProps = await blob.GetAccountPropertiesAsync();

        // Display the properties.
        Console.WriteLine("Account properties");
        Console.WriteLine("  AccountKind: {0}", acctProps.AccountKind);
        Console.WriteLine("      SkuName: {0}", acctProps.SkuName);
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre outras operações que você pode executar em uma conta de armazenamento por meio do [portal do Azure](https://portal.azure.com) e da API REST do Azure.

- [Operação obter informações da conta (REST)](/rest/api/storageservices/get-account-information)
