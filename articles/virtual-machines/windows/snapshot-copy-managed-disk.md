---
title: Créer une capture instantanée d’un disque dur virtuel dans Azure
description: Découvrez comment créer une copie d’une machine virtuelle Azure pour l’utiliser comme sauvegarde ou pour la résolution de problèmes.
author: roygara
manager: twooley
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 10/08/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 92957bd078c04a9bb7ac35f9d30f042a44e10764
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82100632"
---
# <a name="create-a-snapshot"></a>Créer un instantané

Une capture instantanée est une copie complète en lecture seule d’un disque dur virtuel (VHD). Vous pouvez prendre une capture instantanée d’un disque dur virtuel de système d’exploitation ou de données que vous utiliserez comme sauvegarde ou pour résoudre les problèmes de la machine virtuelle.

Si vous prévoyez d’utiliser la capture instantanée pour créer une machine virtuelle, nous vous recommandons d’arrêter convenablement la machine virtuelle avant de prendre une capture instantanée, de façon à mettre fin à tous les processus en cours.

## <a name="use-the-azure-portal"></a>Utilisation du portail Azure 

Pour créer une capture instantanée, effectuez les étapes suivantes : 
1.  Dans le [portail Azure](https://portal.azure.com), sélectionnez **Créer une ressource**.
2. Recherchez et sélectionnez **Capture instantanée**.
3. Dans la fenêtre **Capture instantanée**, sélectionnez **Créer**. La fenêtre **Créer une capture instantanée** s’affiche.
4. Entrez un **nom** pour la capture instantanée.
5. Sélectionnez un [groupe de ressources](../../azure-resource-manager/management/overview.md#resource-groups) existant ou entrez le nom d’un nouveau. 
6. Sélectionnez un **Emplacement** de centre de données Azure.  
7. Dans **Disque source**, sélectionnez le disque managé dont vous souhaitez obtenir une capture instantanée.
8. Sélectionnez le **type de compte** à utiliser pour stocker la capture instantanée. Sélectionnez **Standard_HDD**, sauf si vous avez besoin que la capture soit stockée sur un disque hautes performances.
9. Sélectionnez **Create** (Créer).

## <a name="use-powershell"></a>Utiliser PowerShell

Les étapes suivantes montrent comment copier le disque dur virtuel et créer la configuration de capture instantanée. Vous pouvez ensuite prendre une capture instantanée du disque en utilisant l’applet de commande [New-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/new-azsnapshot). 

 

1. Définissez certains paramètres : 

   ```azurepowershell-interactive
   $resourceGroupName = 'myResourceGroup' 
   $location = 'eastus' 
   $vmName = 'myVM'
   $snapshotName = 'mySnapshot'  
   ```

2. Obtenez la machine virtuelle :

   ```azurepowershell-interactive
   $vm = get-azvm `
   -ResourceGroupName $resourceGroupName 
   -Name $vmName
   ```

3. Créez la configuration de capture instantanée. Pour cet exemple, la capture instantanée est celle du disque de système d’exploitation :

   ```azurepowershell-interactive
   $snapshot =  New-AzSnapshotConfig 
   -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id 
   -Location $location 
   -CreateOption copy
   ```
   
   > [!NOTE]
   > Si vous souhaitez stocker votre capture instantanée dans le stockage résilient aux zones, créez-la dans une région qui prend en charge des [zones de disponibilité](../../availability-zones/az-overview.md) et inclut le paramètre `-SkuName Standard_ZRS`.   
   
4. Prenez la capture instantanée :

   ```azurepowershell-interactive
   New-AzSnapshot 
   -Snapshot $snapshot 
   -SnapshotName $snapshotName 
   -ResourceGroupName $resourceGroupName 
   ```


## <a name="next-steps"></a>Étapes suivantes

Créez une machine virtuelle à partir d’une capture instantanée en créant un disque managé à partir d’une capture instantanée, puis en attachant le nouveau disque managé comme disque de système d’exploitation. Pour plus d’informations, consultez l’exemple dans [Créer une machine virtuelle à partir d’une capture instantanée avec PowerShell](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json).
