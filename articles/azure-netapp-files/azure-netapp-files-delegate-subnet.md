---
title: Déléguer un sous-réseau à Azure NetApp Files | Microsoft Docs
description: Décrit la procédure de délégation d’un sous-réseau à Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: b-juche
ms.openlocfilehash: b83f530549ffa43789963fd0c95b4982f5289356
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80054461"
---
# <a name="delegate-a-subnet-to-azure-netapp-files"></a>Déléguer un sous-réseau à Azure NetApp Files 

Vous devez déléguer un sous-réseau à Azure NetApp Files.   Lorsque vous créez un volume, vous devez spécifier le sous-réseau délégué.

## <a name="considerations"></a>Considérations
* L’Assistant de création d’un sous-réseau applique par défaut un masque réseau /24, qui fournit 251 adresses IP disponibles. L’utilisation d’un masque réseau /28, qui fournit 16 adresses IP utilisables, est suffisante pour le service.
* Dans chaque réseau virtuel Azure, un seul sous-réseau peut être délégué à Azure NetApp Files.   
   Azure vous permet de créer plusieurs sous-réseaux délégués dans un réseau virtuel.  Cependant, toute tentative de création d’un nouveau volume échoue si vous utilisez plusieurs sous-réseaux délégués.
* Vous ne pouvez pas désigner un groupe de sécurité ou un point de terminaison de service réseau dans le sous-réseau délégué. Cette opération fait échouer la délégation du sous-réseau.
* L’accès à un volume depuis un réseau virtuel homologué globalement n’est pas pris en charge actuellement.
* La création d’[itinéraires personnalisés définis par l’utilisateur](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes) sur des sous-réseaux de machines virtuelles avec un préfixe d’adresse (destination) vers un sous-réseau délégué à Azure NetApp Files n’est pas prise en charge. Cela affecte la connectivité des machines virtuelles.

## <a name="steps"></a>Étapes 
1.  Accédez au panneau **Réseaux virtuels** dans le Portail Azure, puis sélectionnez le réseau virtuel que vous souhaitez utiliser pour Azure NetApp Files.    

1. Sélectionnez **Sous-réseaux** dans le panneau Réseau virtuel et cliquez sur le bouton **+Sous-réseau**. 

1. Créez un sous-réseau à utiliser pour Azure NetApp Files en renseignant les champs obligatoires suivants dans la page Ajouter un sous-réseau :
    * **Name** : spécifiez le nom du sous-réseau.
    * **Plage d’adresses** : spécifiez la plage d’adresses IP.
    * **Délégation de sous-réseau** : sélectionnez **Microsoft.NetApp/volumes**. 

      ![Délégation de sous-réseau](../media/azure-netapp-files/azure-netapp-files-subnet-delegation.png)
    
Vous pouvez également créer et déléguer un sous-réseau lorsque vous [créez un volume pour Azure NetApp Files](azure-netapp-files-create-volumes.md). 

## <a name="next-steps"></a>Étapes suivantes  
* [Créer un volume pour Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [En savoir plus sur l’intégration d’un réseau virtuel pour les services Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)


