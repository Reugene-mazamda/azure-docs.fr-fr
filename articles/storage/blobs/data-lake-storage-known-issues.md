---
title: Problèmes connus avec Azure Data Lake Storage Gen2 | Microsoft Docs
description: Apprenez-en davantage sur les limitations et les problèmes connus d’Azure Data Lake Storage Gen2.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: dfa4d65464192b90d4a6f74255faaf8b664ce118
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81767964"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Problèmes connus avec Azure Data Lake Storage Gen2

Cet article décrit les limitations et les problèmes connus d’Azure Data Lake Storage Gen2.

## <a name="supported-blob-storage-features"></a>Fonctionnalités du stockage Blob prises en charge

Les comptes ayant un espace de noms hiérarchique prennent en charge un nombre croissant de fonctionnalités de stockage Blob. Pour en obtenir la liste complète, consultez [Fonctionnalités de stockage blob disponibles dans Azure Data Lake Storage Gen2](data-lake-storage-supported-blob-storage-features.md).

## <a name="supported-azure-service-integrations"></a>Intégrations de service Azure prises en charge

Azure Data Lake Storage Gen2 prend en charge plusieurs services Azure permettant d’ingérer des données, d’obtenir des données d’analytique et de créer des représentations visuelles. Pour obtenir la liste des services Azure pris en charge, consultez [Services Azure prenant en charge Azure Data Lake Storage Gen2](data-lake-storage-supported-azure-services.md).

Consultez [Services Azure prenant en charge Azure Data Lake Storage Gen2](data-lake-storage-supported-azure-services.md).

## <a name="supported-open-source-platforms"></a>Plateformes open source prises en charge

Plusieurs plateformes open source prennent en charge le stockage Data Lake Gen2. Pour obtenir une liste complète, consultez [Plateformes open source prenant en charge Azure Data Lake Storage Gen2](data-lake-storage-supported-open-source-platforms.md).

Consultez [Plateformes open source prenant en charge Azure Data Lake Storage Gen2](data-lake-storage-supported-open-source-platforms.md).

## <a name="blob-storage-apis"></a>API Stockage Blob

Les API Blob et les API Data Lake Storage Gen2 peuvent fonctionner sur les mêmes données.

Cette section décrit les problèmes et les limitations liés à l’utilisation des API d’objets BLOB et des API Data Lake Storage Gen 2 pour fonctionner sur les mêmes données.

* Vous ne pouvez pas utiliser à la fois les API d’objets BLOB et les API Data Lake Storage pour écrire dans la même instance d’un fichier. Si vous écrivez dans un fichier à l’aide des API Data Lake Storage Gen 2, les blocs de ce fichier ne seront pas visibles pour les appels à l’API [Obtenir la liste de bloc](https://docs.microsoft.com/rest/api/storageservices/get-block-list) d’objets BLOB. Vous pouvez remplacer un fichier à l’aide des API Data Lake Storage Gen 2 ou des API d’objets BLOB. Cela n’affecte pas les propriétés du fichier.

* Lorsque vous utilisez l’opération [Lister les objets BLOB](https://docs.microsoft.com/rest/api/storageservices/list-blobs) sans spécifier de délimiteur, les résultats incluront à la fois des répertoires et des objets BLOB. Si vous choisissez d’utiliser un délimiteur, n’utilisez qu’une barre oblique (`/`). Il s’agit du seul délimiteur pris en charge.

* Si vous utilisez l’API [Supprimer un objet BLOB](https://docs.microsoft.com/rest/api/storageservices/delete-blob) pour supprimer un répertoire, ce répertoire est supprimé uniquement s’il est vide. Cela signifie que vous ne pouvez pas utiliser les répertoires de suppression de l’API d’objet BLOB de manière récursive.

Ces API REST BLOB ne sont pas prises en charge :

* [Placer BLOB (Page)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Put Page](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [Obtenir les portées de page](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges)
* [Copie incrémentielle BLOB](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob)
* [Placer la page à partir de l’URL](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)
* [Placer BLOB (ajouter)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Append Block](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [Ajouter un bloc à partir d’une URL](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url)

Les disques de machine virtuelle non gérés ne sont pas pris en charge dans les comptes qui ont un espace de noms hiérarchique. Si vous souhaitez activer un espace de noms hiérarchique sur un compte de stockage, placez les disques de machine virtuelle non gérés dans un compte de stockage pour lequel la fonctionnalité espace de noms hiérarchique n’est pas activée.

<a id="api-scope-data-lake-client-library" />

## <a name="file-system-support-in-sdks-powershell-and-azure-cli"></a>Prise en charge des systèmes de fichiers dans les SDK, PowerShell et Azure CLI

- Actuellement, les opérations d’obtention et de définition de listes de contrôle d’accès ne sont pas récursives.
- La prise en charge pour [Azure CLI](data-lake-storage-directory-file-acl-cli.md) est en préversion publique.


## <a name="lifecycle-management-policies"></a>Stratégies de gestion du cycle de vie

* La suppression des instantanés d’objets BLOB n’est pas encore prise en charge.  

## <a name="archive-tier"></a>Niveau Archive

Il existe actuellement un bogue qui affecte le niveau d’accès Archive.


## <a name="blobfuse"></a>Blobfuse

Blobfuse n’est pas pris en charge.

<a id="known-issues-tools" />

## <a name="azcopy"></a>AzCopy

Utilisez uniquement la dernière version d’AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). Les versions antérieures d’AzCopy (telles qu’AzCopy v8.1) ne sont pas prises en charge.

<a id="storage-explorer" />

## <a name="azure-storage-explorer"></a>Explorateur de stockage Azure

Utilisez uniquement les versions `1.6.0` ou ultérieures.

<a id="explorer-in-portal" />

## <a name="storage-explorer-in-the-azure-portal"></a>Explorateur Stockage dans le portail Azure

Les listes de contrôle d’accès ne sont pas prises en charge pour le moment.

<a id="third-party-apps" />

## <a name="thirdpartyapplications"></a>Applications tierces

Les applications tierces qui utilisent les API REST continueront à fonctionner si vous les utilisez avec Data Lake Storage Gen2. Les applications qui appellent des API Blob fonctionneront probablement.

## <a name="access-control-lists-acl-and-anonymous-read-access"></a>Listes de contrôle d’accès (ACL) et accès en lecture anonyme

Si l’[accès en lecture anonyme](storage-manage-access-to-resources.md) a été accordé à un conteneur, les listes de contrôle d’accès n’ont aucun effet sur ce conteneur ou les fichiers de ce conteneur.

## <a name="windows-azure-storage-blob-wasb-driver-unsupported-with-data-lake-storage-gen2"></a>Pilote Windows Azure Storage Blob (WASB) (non pris en charge avec Data Lake Storage Gen2)

Actuellement, le pilote WASB, qui a été conçu pour fonctionner avec l’API Blob uniquement, rencontre des problèmes dans quelques scénarios courants. C’est le cas en particulier quand il s’agit d’un client pour un compte de stockage prenant en charge un espace de noms hiérarchique. L’accès multiprotocole sur Data Lake Storage n’atténue pas ces problèmes. 

Pour le moment (et probablement pour très longtemps), nous ne prenons pas en charge les utilisateurs qui utilisent le pilote WASB en tant que client pour un compte de stockage prenant en charge un espace de noms hiérarchique. Nous vous recommandons plutôt d’utiliser le pilote [Azure Blob File System (ABFS)](data-lake-storage-abfs-driver.md) dans votre environnement Hadoop. Si vous tentez d’effectuer une migration à partir d’un environnement Hadoop local avec une version antérieure à Hadoop Branch-3, ouvrez un ticket de support Azure pour que nous puissions vous contacter et vous indiquer la bonne direction pour vous et votre organisation.
