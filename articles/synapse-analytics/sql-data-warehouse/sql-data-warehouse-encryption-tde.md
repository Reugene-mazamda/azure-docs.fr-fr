---
title: Transparent Data Encryption (Portail)
description: Transparent Data Encryption (TDE) dans Azure Synapse Analytics
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/30/2019
ms.author: jrasnick
ms.reviewer: rortloff
ms.custom: seo-lt-2019
ms.openlocfilehash: 247691326e3aa2c8027dd0318b23a2cbfcba1efe
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80745231"
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Prise en main du chiffrement transparent des données (TDE)

> [!div class="op_single_selector"]
>
> * [Présentation de la sécurité](sql-data-warehouse-overview-manage-security.md)
> * [Authentification](sql-data-warehouse-authentication.md)
> * [Chiffrement (portail)](sql-data-warehouse-encryption-tde.md)
> * [Chiffrement (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permissions"></a>Autorisations requises

Pour activer le chiffrement transparent des données (TDE), vous devez être un administrateur ou un membre du rôle dbmanager.

## <a name="enabling-encryption"></a>Activation du chiffrement

Pour activer TDE, procédez comme suit :

1. Ouvrez la base de données dans le [portail Azure](https://portal.azure.com)
2. Dans le panneau de la base de données, cliquez sur le bouton **Paramètres**
3. Sélectionnez l’option **Chiffrement transparent des données** ![paramètres du portail](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png)
4. Sélectionnez le paramètre **Activé** ![paramètres du portail, activé](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png)
5. Sélectionnez **Enregistrer**
   ![paramètres du portail, enregistrer](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png)  

## <a name="disabling-encryption"></a>Désactivation du chiffrement

Pour désactiver TDE, procédez comme suit :

1. Ouvrez la base de données dans le [portail Azure](https://portal.azure.com)
2. Dans le panneau de la base de données, cliquez sur le bouton **Paramètres**
3. Sélectionnez l’option **Chiffrement transparent des données** ![paramètres du portail](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png)
4. Sélectionnez le paramètre **Désactivé** ![paramètres du portail, désactivé](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png)
5. Sélectionnez **Enregistrer**
   ![paramètres du portail, enregistrer 2](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png)  

## <a name="encryption-dmvs"></a>DMV de chiffrement

Le chiffrement peut être vérifié avec les vues DMV suivantes :

* [sys.databases](/sql/relational-databases/system-catalog-views/sys-databases-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
* [sys.dm_pdw_nodes_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
