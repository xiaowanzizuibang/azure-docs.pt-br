---
title: Definir várias instâncias de uma propriedade
description: Use a operação de cópia em um modelo de Azure Resource Manager para iterar várias vezes ao criar uma propriedade em um recurso.
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: 831ae1af202a1cdf52bdd2bdf0d9a042a97ba52f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81391335"
---
# <a name="property-iteration-in-arm-templates"></a>Iteração de propriedade em modelos ARM

Este artigo mostra como criar mais de uma instância de uma propriedade em seu modelo de Azure Resource Manager (ARM). Ao adicionar o elemento **copiar** à seção Propriedades de um recurso em seu modelo, você pode definir dinamicamente o número de itens para uma propriedade durante a implantação. Você também evita a repetição da sintaxe do modelo.

Você também pode usar a cópia com [recursos](copy-resources.md), [variáveis](copy-variables.md)e [saídas](copy-outputs.md).

## <a name="property-iteration"></a>Iteração de propriedade

O elemento Copy tem o seguinte formato geral:

```json
"copy": [
  {
    "name": "<name-of-loop>",
    "count": <number-of-iterations>,
    "input": <values-for-the-property>
  }
]
```

Para **nome**, forneça o nome da propriedade de recurso que você deseja criar.

A propriedade **Count** especifica o número de iterações que você deseja para a propriedade.

A propriedade de **entrada** especifica as propriedades que você deseja repetir. Você cria uma matriz de elementos construídos com base no valor na propriedade de **entrada** .

O exemplo a seguir mostra como aplicar `copy` para a propriedade dataDisks em uma máquina virtual:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberOfDataDisks": {
      "type": "int",
      "minValue": 0,
      "maxValue": 16,
      "defaultValue": 16,
      "metadata": {
        "description": "The number of dataDisks to create."
      }
    },
    ...
  },
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2017-03-30",
      ...
      "properties": {
        "storageProfile": {
          ...
          "copy": [
            {
              "name": "dataDisks",
              "count": "[parameters('numberOfDataDisks')]",
              "input": {
                "diskSizeGB": 1023,
                "lun": "[copyIndex('dataDisks')]",
                "createOption": "Empty"
              }
            }
          ]
        }
      }
    }
  ]
}
```

Observe que ao usar `copyIndex` dentro de uma iteração de propriedade, você deve fornecer o nome da iteração. A iteração de propriedade também dá suporte a um argumento offset. O deslocamento deve vir após o nome da iteração, como copyIndex (' datadisks ', 1).

Gerenciador de recursos expande a matriz `copy` durante a implantação. O nome da matriz se torna o nome da propriedade. Os valores de entrada se tornam as propriedades do objeto. O modelo implantado se torna:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
        {
          "lun": 0,
          "createOption": "Empty",
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "createOption": "Empty",
          "diskSizeGB": 1023
        },
        {
          "lun": 2,
          "createOption": "Empty",
          "diskSizeGB": 1023
        }
      ],
      ...
```

A operação de cópia é útil ao trabalhar com matrizes porque você pode percorrer cada elemento da matriz. Use a função `length` na matriz para especificar a contagem de iterações e `copyIndex` para recuperar o índice atual na matriz.

O modelo de exemplo a seguir cria um grupo de failover para bancos de dados que são passados como uma matriz.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "primaryServerName": {
            "type": "string"
        },
        "secondaryServerName": {
            "type": "string"
        },
        "databaseNames": {
            "type": "array",
            "defaultValue": [
                "mydb1",
                "mydb2",
                "mydb3"
            ]
        }
    },
    "variables": {
        "failoverName": "[concat(parameters('primaryServerName'),'/', parameters('primaryServerName'),'failovergroups')]"
    },
    "resources": [
        {
            "type": "Microsoft.Sql/servers/failoverGroups",
            "apiVersion": "2015-05-01-preview",
            "name": "[variables('failoverName')]",
            "properties": {
                "readWriteEndpoint": {
                    "failoverPolicy": "Automatic",
                    "failoverWithDataLossGracePeriodMinutes": 60
                },
                "readOnlyEndpoint": {
                    "failoverPolicy": "Disabled"
                },
                "partnerServers": [
                    {
                        "id": "[resourceId('Microsoft.Sql/servers', parameters('secondaryServerName'))]"
                    }
                ],
                "copy": [
                    {
                        "name": "databases",
                        "count": "[length(parameters('databaseNames'))]",
                        "input": "[resourceId('Microsoft.Sql/servers/databases', parameters('primaryServerName'), parameters('databaseNames')[copyIndex('databases')])]"
                    }
                ]
            }
        }
    ],
    "outputs": {
    }
}
```

O elemento de cópia está em uma matriz, assim você pode especificar mais de uma propriedade para o recurso.

```json
{
  "type": "Microsoft.Network/loadBalancers",
  "apiVersion": "2017-10-01",
  "name": "exampleLB",
  "properties": {
    "copy": [
      {
        "name": "loadBalancingRules",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      },
      {
        "name": "probes",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      }
    ]
  }
}
```

Você pode usar iteração de recurso e propriedade juntos. Referência a iteração de propriedade por nome.

```json
{
  "type": "Microsoft.Network/virtualNetworks",
  "apiVersion": "2018-04-01",
  "name": "[concat(parameters('vnetname'), copyIndex())]",
  "copy":{
    "count": 2,
    "name": "vnetloop"
  },
  "location": "[resourceGroup().location]",
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "[parameters('addressPrefix')]"
      ]
    },
    "copy": [
      {
        "name": "subnets",
        "count": 2,
        "input": {
          "name": "[concat('subnet-', copyIndex('subnets'))]",
          "properties": {
            "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
          }
        }
      }
    ]
  }
}
```

## <a name="copy-limits"></a>Limites de cópia

A contagem não pode exceder 800.

A contagem não pode ser um número negativo. Se você implantar um modelo com Azure PowerShell 2,6 ou posterior, CLI do Azure 2.0.74 ou posterior, ou a API REST versão **2019-05-10** ou posterior, poderá definir Count como zero. As versões anteriores do PowerShell, da CLI e da API REST não dão suporte a zero para contagem.

## <a name="example-templates"></a>Modelos de exemplo

O exemplo a seguir mostra um cenário comum para criar mais de um valor para uma propriedade.

|Modelo  |Descrição  |
|---------|---------|
|[Implantação da VM com um número variável de discos de dados](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Implanta vários discos de dados com uma máquina virtual. |

## <a name="next-steps"></a>Próximas etapas

* Para percorrer um tutorial, consulte [tutorial: criar várias instâncias de recursos usando modelos ARM](template-tutorial-create-multiple-instances.md).
* Para outros usos do elemento copiar, consulte:
  * [Iteração de recurso em modelos ARM](copy-resources.md)
  * [Iteração variável em modelos ARM](copy-variables.md)
  * [Iteração de saída em modelos ARM](copy-outputs.md)
* Se você quiser saber mais sobre as seções de um modelo, consulte [criação de modelos de ARM](template-syntax.md).
* Para saber como implantar seu modelo, consulte [implantar um aplicativo com o modelo ARM](deploy-powershell.md).

