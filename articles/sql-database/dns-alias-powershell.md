---
title: Alias PowerShell pour DNS
description: Les applets de commande PowerShell, comme New-AzSqlServerDNSAlias, permettent de rediriger les nouvelles connexions clientes vers un autre serveur Azure SQL Database, sans toucher aux configurations clientes.
keywords: base de données sql dns
ms.custom: seo-lt-2019
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.devlang: PowerShell
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: genemi, amagarwa, maboja, jrasnick, vanto
ms.date: 05/14/2019
ms.openlocfilehash: 9232a99ddd29201e6743c09455d79e9ba22b3b9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74420402"
---
# <a name="powershell-for-dns-alias-to-azure-sql-database"></a>PowerShell pour les alias DNS sur Azure SQL Database

Cet article comporte un script PowerShell montrant une manière de gérer un alias DNS pour Azure SQL Database.

> [!NOTE]
> Cet article a été mis à jour pour tenir compte de l’utilisation du module Az d’Azure PowerShell ou d’Azure CLI. Vous pouvez toujours utiliser le module AzureRM, qui continue à recevoir des correctifs de bogues jusqu’à au moins décembre 2020.
>
> Pour en savoir plus sur le module Az et la compatibilité avec AzureRM, consultez [Présentation du module Az d’Azure PowerShell](/powershell/azure/new-azureps-module-az). Pour obtenir des instructions d’installation, consultez [Installer Azure PowerShell](/powershell/azure/install-az-ps) ou [Installer Azure CLI](/cli/azure/install-azure-cli).

## <a name="dns-alias-in-connection-string"></a>Alias DNS dans la chaîne de connexion

Pour connecter un serveur Azure SQL Database en particulier, un client tel que SQL Server Management Studio (SSMS) peut fournir le nom d’alias DNS au lieu du vrai nom du serveur. Dans l’exemple de chaîne de serveur suivant, l’alias *any-unique-alias-name* remplace le premier nœud délimité par un point dans la chaîne de serveur à quatre nœuds :

   `<yourServer>.database.windows.net`

## <a name="prerequisites"></a>Prérequis

Pour exécuter le script PowerShell de démonstration proposé dans cet article, les prérequis suivants s’appliquent :

- Un abonnement et un compte Azure, pour un essai gratuit, consultez les [Versions d’essai Azure](https://azure.microsoft.com/free/)
- Deux serveurs Azure SQL Database

## <a name="example"></a>Exemple

L’exemple de code suivant commence par affecter des valeurs littérales à plusieurs variables.

Pour exécuter le code, modifiez les valeurs des espaces réservés pour les faire correspondre aux valeurs réelles de votre système.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Les applets de commande utilisées sont les suivantes :

- [New-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/New-azSqlServerDnsAlias) : crée un alias DNS dans le système de service Azure SQL Database. L’alias fait référence au serveur de base de données 1.
- [Get-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlServerDnsAlias) : obtient et liste tous les alias attribués au serveur 1 SQL DB.
- [Set-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Set-azSqlServerDnsAlias) : modifie le nom du serveur que l’alias doit référencer, de serveur 1 à serveur 2.
- [Remove-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Remove-azSqlServerDnsAlias) : supprime l’alias du serveur de base de données 2, en utilisant le nom de l’alias.

Pour installer ou mettre à niveau, consultez [Installer le module Azure PowerShell](/powershell/azure/install-az-ps).

Utilisez `Get-Module -ListAvailable Az` dans *powershell\_ise.exe* pour trouver la version.

```powershell
$subscriptionName = '<subscriptionName>';
$sqlServerDnsAliasName = '<aliasName>';
$resourceGroupName = '<resourceGroupName>';  
$sqlServerName = '<sqlServerName>';
$resourceGroupName2 = '<resourceGroupNameTwo>'; # can be same or different than $resourceGroupName
$sqlServerName2 = '<sqlServerNameTwo>'; # must be different from $sqlServerName.

# login to Azure
Connect-AzAccount -SubscriptionName $subscriptionName;
$subscriptionId = Get-AzSubscription -SubscriptionName $subscriptionName;

Write-Host 'Assign an alias to server 1...';
New-AzSqlServerDnsAlias –ResourceGroupName $resourceGroupName -ServerName $sqlServerName `
    -Name $sqlServerDnsAliasName;

Write-Host 'Get the aliases assigned to server 1...';
Get-AzSqlServerDnsAlias –ResourceGroupName $resourceGroupName -ServerName $sqlServerName;

Write-Host 'Move the alias from server 1 to server 2...';
Set-AzSqlServerDnsAlias –ResourceGroupName $resourceGroupName2 -TargetServerName $sqlServerName2 `
    -Name $sqlServerDnsAliasName `
    -SourceServerResourceGroup $resourceGroupName -SourceServerName $sqlServerName `
    -SourceServerSubscriptionId $subscriptionId.Id;

Write-Host 'Get the aliases assigned to server 2...';
Get-AzSqlServerDnsAlias –ResourceGroupName $resourceGroupName2 -ServerName $sqlServerName2;

Write-Host 'Remove the alias from server 2...';
Remove-AzSqlServerDnsAlias –ResourceGroupName $resourceGroupName2 -ServerName $sqlServerName2 `
    -Name $sqlServerDnsAliasName;
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Les commandes utilisées sont les suivantes :

- [az sql server dns-alias create](https://docs.microsoft.com/powershell/module/az.Sql/New-azSqlServerDnsAlias) : crée un alias DNS dans le système de service Azure SQL Database. L’alias fait référence au serveur de base de données 1.
- [az sql server dns-alias show](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlServerDnsAlias) : obtient et liste tous les alias attribués au serveur 1 SQL DB.
- [az sql server dns-alias set](https://docs.microsoft.com/powershell/module/az.Sql/Set-azSqlServerDnsAlias) : modifie le nom du serveur que l’alias doit référencer, de serveur 1 à serveur 2.
- [az sql server dns-alias delete](https://docs.microsoft.com/powershell/module/az.Sql/Remove-azSqlServerDnsAlias) : supprime l’alias du serveur de base de données 2, en utilisant le nom de l’alias.

Pour installer ou mettre à niveau Azure CLI, consultez [Installer Azure CLI](/cli/azure/install-azure-cli).

```azurecli-interactive
$subscriptionName = '<subscriptionName>';
$sqlServerDnsAliasName = '<aliasName>';
$resourceGroupName = '<resourceGroupName>';  
$sqlServerName = '<sqlServerName>';
$resourceGroupName2 = '<resourceGroupNameTwo>'; # can be same or different than $resourceGroupName
$sqlServerName2 = '<sqlServerNameTwo>'; # must be different from $sqlServerName.

# login to Azure
az login -SubscriptionName $subscriptionName;
$subscriptionId = az account list[0].i -SubscriptionName $subscriptionName;

Write-Host 'Assign an alias to server 1...';
az sql server dns-alias create –-resource-group $resourceGroupName --server $sqlServerName `
    --name $sqlServerDnsAliasName;

Write-Host 'Get the aliases assigned to server 1...';
az sql server dns-alias show –-resource-group $resourceGroupName --server $sqlServerName;

Write-Host 'Move the alias from server 1 to server 2...';
az sql server dns-alias set –-resource-group $resourceGroupName2 --server $sqlServerName2 `
    --name $sqlServerDnsAliasName `
    --original-resource-group $resourceGroupName --original-server $sqlServerName `
    --original-subscription-id $subscriptionId.Id;

Write-Host 'Get the aliases assigned to server 2...';
az sql server dns-alias show –-resource-group $resourceGroupName2 --server $sqlServerName2;

Write-Host 'Remove the alias from server 2...';
az sql server dns-alias delete –-resource-group $resourceGroupName2 --server $sqlServerName2 `
    --name $sqlServerDnsAliasName;
```

* * *

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir une description complète de la fonctionnalité d’alias DNS pour SQL Database, consultez [Alias DNS pour Azure SQL Database](dns-alias-overview.md).
