---
title: Événement de début de redimensionnement de pool Azure Batch
description: Référence pour l’événement de début de redimensionnement de pool Batch. L’exemple montre le corps d’un événement de début de redimensionnement de pool, pour un redimensionnement de pool de 0 à 2 en mode manuel.
ms.topic: article
ms.date: 04/20/2017
ms.author: labrenne
ms.openlocfilehash: b4aa503c5dfbe00a77216277bdaf7e4c0dc3b1bd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82115871"
---
# <a name="pool-resize-start-event"></a>Événement de début de redimensionnement de pool

 Cet événement est émis quand un redimensionnement de pool a commencé. Étant donné que le redimensionnement de pool est un événement asynchrone, vous pouvez vous attendre à ce qu’un événement de fin de redimensionnement de pool soit émis au terme de l’opération de redimensionnement.

 L’exemple suivant montre le corps d’un événement de début de redimensionnement de pool pour redimensionnement de pool de 0 à 2 en mode manuel.

```
{
    "id": "myPool1",
    "nodeDeallocationOption": "Invalid",
    "currentDedicatedNodes": 0,
    "targetDedicatedNodes": 2,
    "currentLowPriorityNodes": 0,
    "targetLowPriorityNodes": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|Élément|Type|Notes|
|-------------|----------|-----------|
|`id`|String|ID du pool.|
|`nodeDeallocationOption`|String|Spécifie quand des nœuds peuvent être supprimés du pool en cas de diminution de la taille du pool.<br /><br /> Les valeurs possibles sont les suivantes :<br /><br /> **requeue** : arrêter les tâches en cours d’exécution et les replacer en file d’attente. Les tâches sont ré-exécutées lors de l’activation du travail. Supprimez les nœuds dès que les tâches sont terminées.<br /><br /> **terminate** : mettre fin aux tâches en cours d’exécution. Les tâches ne sont pas ré-exécutées. Supprimez les nœuds dès que les tâches sont terminées.<br /><br /> **taskcompletion** : autoriser l’achèvement des tâches en cours d’exécution. Ne planifiez aucune nouvelle tâche en attendant. Supprimer les nœuds quand toutes les tâches sont terminées.<br /><br /> **Retaineddata** : autoriser l’achèvement des tâches en cours d’exécution, puis attendre que toutes les périodes de rétention des données expirent. Ne planifiez aucune nouvelle tâche en attendant. Supprimez les nœuds une fois que toutes les périodes de rétention ont expiré.<br /><br /> La valeur par défaut est requeue.<br /><br /> Si la taille du pool augmente, cela signifie que la valeur est définie **invalide**.|
|`currentDedicatedNodes`|Int32|Nombre de nœuds de calcul actuellement affectés au pool.|
|`targetDedicatedNodes`|Int32|Nombre de nœuds de calcul demandés pour le pool.|
|`currentLowPriorityNodes`|Int32|Nombre de nœuds de calcul actuellement affectés au pool.|
|`targetLowPriorityNodes`|Int32|Nombre de nœuds de calcul demandés pour le pool.|
|`enableAutoScale`|Bool|Spécifie si la taille du pool s’ajuste automatiquement au fil du temps.|
|`isAutoPool`|Bool|Spécifie si le pool a été créé via un mécanisme AutoPool du travail.|
