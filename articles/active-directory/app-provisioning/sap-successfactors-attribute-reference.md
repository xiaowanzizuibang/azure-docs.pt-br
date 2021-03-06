---
title: Referência de atributo SAP SuccessFactors | Microsoft Docs
description: Saiba quais atributos de SuccessFactors são suportados pelo provisionamento controlado por SuccessFactors-HR
services: active-directory
author: cmmdesai
documentationcenter: na
manager: jodadzie
ms.assetid: afb77f2d-5ddd-4c2e-a840-09021b0efef1
ms.service: active-directory
ms.subservice: app-provisioning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2019
ms.author: chmutali
ms.openlocfilehash: 00b16f969525e7b802c008ba247ecba015875689
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77522349"
---
# <a name="sap-successfactors-attribute-reference"></a>Referência de atributo SAP SuccessFactors

## <a name="supported-successfactors-entities-and-attributes"></a>Atributos e entidades SuccessFactors com suporte

A tabela a seguir captura a lista de atributos SuccessFactors com suporte nos dois aplicativos de provisionamento a seguir: 
* [SuccessFactors para Active Directory provisionamento de usuário](../saas-apps/sap-successfactors-inbound-provisioning-tutorial.md)
* [SuccessFactors para provisionamento de usuário do Azure AD](../saas-apps/sap-successfactors-inbound-provisioning-cloud-only-tutorial.md) 

| \# | Entidade SuccessFactors                  | Atributo SuccessFactors     | Tipo de operação |
|----|----------------------------------------|------------------------------|----------------|
| 1  | Perpessoa                              | personIdExternal             | Ler           |
| 2  | Perpessoa                              | personId                     | Ler           |
| 3  | Perpessoa                              | perPersonUuid                | Ler           |
| 4  | Entre pessoas                            | displayName                  | Ler           |
| 5  | Entre pessoas                            | firstName                    | Ler           |
| 6  | Entre pessoas                            | gender                       | Ler           |
| 7  | Entre pessoas                            | lastName                     | Ler           |
| 8  | Entre pessoas                            | middleName                   | Ler           |
| 9  | Entre pessoas                            | preferência                | Ler           |
| 10 | Usuário                                   | addressLine1                 | Ler           |
| 11 | Usuário                                   | Endereço2                 | Ler           |
| 12 | Usuário                                   | addressLIne3                 | Ler           |
| 13 | Usuário                                   | Ficha                | Ler           |
| 14 | Usuário                                   | Celular                    | Ler           |
| 15 | Usuário                                   | city                         | Ler           |
| 16 | Usuário                                   | country                      | Ler           |
| 17 | Usuário                                   | custom01                     | Ler           |
| 18 | Usuário                                   | custom02                     | Ler           |
| 19 | Usuário                                   | custom03                     | Ler           |
| 20 | Usuário                                   | custom04                     | Ler           |
| 21 | Usuário                                   | custom05                     | Ler           |
| 22 | Usuário                                   | custom06                     | Ler           |
| 23 | Usuário                                   | custom07                     | Ler           |
| 24 | Usuário                                   | custom08                     | Ler           |
| 25 | Usuário                                   | custom09                     | Ler           |
| 26 | Usuário                                   | personalizado10                     | Ler           |
| 27 | Usuário                                   | custom11                     | Ler           |
| 28 | Usuário                                   | custom12                     | Ler           |
| 29 | Usuário                                   | custom13                     | Ler           |
| 30 | Usuário                                   | custom14                     | Ler           |
| 31 | Usuário                                   | empId                        | Ler           |
| 32 | Usuário                                   | homePhone                    | Ler           |
| 33 | Usuário                                   | jobFamily                    | Ler           |
| 34 | Usuário                                   | apelido                     | Ler           |
| 35 | Usuário                                   | state                        | Ler           |
| 36 | Usuário                                   | timeZone                     | Ler           |
| 37 | Usuário                                   | username                     | Ler           |
| 38 | Usuário                                   | zipCode                      | Ler           |
| 39 | Por telefone                               | areaCode                     | Ler           |
| 40 | Por telefone                               | countryCode                  | Ler           |
| 41 | Por telefone                               | extensão                    | Ler           |
| 42 | Por telefone                               | phoneNumber                  | Ler           |
| 43 | Por telefone                               | telefonetype                    | Ler           |
| 44 | Enviar por email                               | emailAddress                 | Leitura, Gravação    |
| 45 | Enviar por email                               | emailtype                    | Ler           |
| 46 | EmpEmployment                          | firstDateWorked              | Ler           |
| 47 | EmpEmployment                          | lastDateWorked               | Ler           |
| 48 | EmpEmployment                          | userId                       | Ler           |
| 49 | EmpEmployment                          | isContingentWorker           | Ler           |
| 50 | EmpJob                                 | countryOfCompany             | Ler           |
| 51 | EmpJob                                 | emplStatus                   | Ler           |
| 52 | EmpJob                                 | endDate                      | Ler           |
| 53 | EmpJob                                 | startDate                    | Ler           |
| 54 | EmpJob                                 | jobTitle                     | Ler           |
| 55 | EmpJob                                 | position                     | Ler           |
| 65 | EmpJob                                 | customString13               | Ler           |
| 56 | EmpJob                                 | managerId                    | Ler           |
| 57 | EmpJob\.BusinessUnit                   | businessUnit                 | Ler           |
| 58 | EmpJob\.BusinessUnit                   | businessUnitId               | Ler           |
| 59 | Empresa\.EmpJob                        | company                      | Ler           |
| 60 | Empresa\.EmpJob                        | companyId                    | Ler           |
| 61 | EmpJob\.Company\.CountryOfRegistration | twoCharCountryCode           | Ler           |
| 62 | EmpJob\.CostCenter                     | costCenter                   | Ler           |
| 63 | EmpJob\.CostCenter                     | costCenterId                 | Ler           |
| 64 | EmpJob\.CostCenter                     | costCenterDescription        | Ler           |
| 65 | Departamento\.de EmpJob                     | department                   | Ler           |
| 66 | Departamento\.de EmpJob                     | departmentId                 | Ler           |
| 67 | Divisão\.de EmpJob                       | division                     | Ler           |
| 68 | Divisão\.de EmpJob                       | divisionId                   | Ler           |
| 69 | EmpJob\.JobCode                        | jobCode                      | Ler           |
| 70 | EmpJob\.JobCode                        | jobCodeId                    | Ler           |
| 71 | Local\.EmpJob                       | LocalName                 | Ler           |
| 72 | Local\.EmpJob                       | officeLocationAddress        | Ler           |
| 73 | Local\.EmpJob                       | officeLocationCity           | Ler           |
| 74 | Local\.EmpJob                       | officeLocationCustomString4  | Ler           |
| 75 | Local\.EmpJob                       | officeLocationZipCode        | Ler           |
| 76 | EmpJob\.PayGrade                       | payGrade                     | Ler           |
| 77 | EmpEmploymentTermination               | activeEmploymentsCount       | Ler           |
| 78 | EmpEmploymentTermination               | latestTerminationDate        | Ler           |


## <a name="default-attribute-mapping"></a>Mapeamento de atributo padrão

A tabela a seguir fornece o mapeamento de atributo padrão entre os atributos SuccessFactors listados acima e os atributos AD/Azure AD. Na folha "mapeamento" do aplicativo de provisionamento do Azure AD, você pode modificar esse mapeamento padrão para incluir atributos da lista acima. 

| \# | Entidade SuccessFactors                  | Atributo SuccessFactors | Mapeamento de atributo padrão AD/AD do Azure   | Comentário de processamento                                                                            |
|----|----------------------------------------|--------------------------|-----------------------------------------|----------------------------------------------------------------------------------------------|
| 1  | Perpessoa                              | personIdExternal         | employeeId                              | Usado como atributo correspondente                                                                   |
| 2  | Perpessoa                              | perPersonUuid            | \[Não mapeado \- usado como âncora de origem\] | Durante a sincronização inicial, o serviço de provisionamento vincula o personUuid a objectGuid\. existentes  |
| 3  | Entre pessoas                            | displayName              | displayName                             | NA                                                                                           |
| 4  | Entre pessoas                            | firstName                | givenName                               | NA                                                                                           |
| 5  | Entre pessoas                            | lastName                 | sn                                      | NA                                                                                           |
| 6  | Usuário                                   | addressLine1             | streetAddress                           | NA                                                                                           |
| 7  | Usuário                                   | city                     | l                                       | NA                                                                                           |
| 8  | Usuário                                   | country                  | co                                      | NA                                                                                           |
| 9  | Usuário                                   | state                    | st                                      | NA                                                                                           |
| 10 | Usuário                                   | username                 | samAccountName                          | NA                                                                                           |
| 11 | Usuário                                   | zipCode                  | postalCode                              | NA                                                                                           |
| 12 | Enviar por email                               | emailAddress             | mail                                    | NA                                                                                           |
| 13 | EmpJob                                 | jobTitle                 | título                                   | NA                                                                                           |
| 14 | EmpJob                                 | managerId                | manager                                 | NA                                                                                           |
| 15 | EmpJob\.Company\.CountryOfRegistration | twoCharCountryCode       | c                                       | NA                                                                                           |
| 16 | Departamento\.de EmpJob                     | department               | department                              | NA                                                                                           |
| 17 | Divisão\.de EmpJob                       | division                 | company                                 | NA                                                                                           |
| 18 | Local\.EmpJob                       | officeLocationAddress    | streetAddress                           | NA                                                                                           |
| 19 | Local\.EmpJob                       | officeLocationZipCode    | postalCode                              | NA                                                                                           |
| 20 | EmpEmploymentTermination               | activeEmploymentsCount   | accountEnabled                          | se activeEmploymentsCount = 0, desabilite o account\.                                           |

